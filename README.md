# Security-Project-02
 ELK, Wazuh, Machine Learning, Cyber Attacks

mkdir wazuh
CD wazuh
wget https://packages.wazuh.com-install.sh
wget https://packages.wazuh.com/4.9/config.yml
sudo su
Chmod +x wazuh -install.sh
nano config.yml
./wazuh-install.sh -i --generate-config-files
ls
./wazuh-install.sh -i --wazuh-indexer node-1
./wazuh-install.sh -i --start-cluster
./wazuh-install.sh -i --wazuh-server wazuh-1
./wazuh-install.sh -i --wazuh-dashboard dashboard
