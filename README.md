# Step-by-Step Wazuh Installation Guide

## 1. Create a Directory for Wazuh
mkdir wazuh
cd wazuh

## 2. Download the Wazuh Installation Script
wget https://packages.wazuh.com-install.sh

## 3. Download config.yml file with the following command:
wget https://packages.wazuh.com/4.9/config.yml

## 4. Switch to Root User
sudo su

## 5. Make the Installation Script Executable
chmod +x wazuh-install.sh

## 6. Edit the Configuration File
nano config.yml

## 7. Generate Configuration Files
./wazuh-install.sh -i --generate-config-files

## 8. List the files to ensure everything has been downloaded and generated correctly:
ls

## 9. Install Wazuh Indexer (node-1)
./wazuh-install.sh -i --wazuh-indexer node-1

## 10. Start the Wazuh Cluster
./wazuh-install.sh -i --start-cluster

# 11. Install Wazuh Server (wazuh-1)
./wazuh-install.sh -i --wazuh-server wazuh-1

# 12. Install Wazuh Dashboard
./wazuh-install.sh -i --wazuh-dashboard dashboard
