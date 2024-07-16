# Utiliser une image de base Ubuntu
FROM ubuntu:latest

# Mettre à jour le système et installer les dépendances nécessaires
RUN apt-get update && apt-get install -y \
    wget \
    gnupg \
    lsb-release \
    apt-transport-https \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Ajouter la clé GPG de Mosquitto
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 61611AE430993623

# Ajouter le dépôt Mosquitto
RUN echo "deb http://repo.mosquitto.org/debian bullseye main" | tee /etc/apt/sources.list.d/mosquitto.list

# Installer Mosquitto
RUN apt-get update \
    && apt-get install -y mosquitto mosquitto-clients \
    && rm -rf /var/lib/apt/lists/*

# Télécharger et installer Wazuh agent
ENV WAZUH_MANAGER='172.18.0.2' \
    WAZUH_AGENT_GROUP='default' \
    WAZUH_AGENT_NAME='mosquitto'
RUN wget https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.8.0-1_amd64.deb \
    && dpkg -i ./wazuh-agent_4.8.0-1_amd64.deb \
    && rm wazuh-agent_4.8.0-1_amd64.deb

# Exposer le port Mosquitto
EXPOSE 1883

# Démarrer les services Mosquitto et Wazuh
CMD ["sh", "-c", "service mosquitto start && service wazuh-agent start && tail -f /dev/null"]

