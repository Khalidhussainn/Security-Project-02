# Step-by-Step Wazuh Installation and Integrating with ELK Stack


### Step 1: Install Prerequisites

   - **Install necessary packages:**
      ```bash
      apt-get install apt-transport-https zip unzip lsb-release curl gnupg
      ```
### Step 2: Elasticsearch Installation


   - **Install the GPG key:**

      ```bash
      curl -s https://artifacts.elastic.co/GPG-KEY-elasticsearch | gpg --no-default-keyring --keyring gnupg-ring:/usr/share/keyrings/elasticsearch.gpg --import && chmod 644 /usr/share/keyrings/elasticsearch.gpg
      ```
   - **Add the Elastic Stack repository:**
   
      ```bash
      echo "deb [signed-by=/usr/share/keyrings/elasticsearch.gpg] https://artifacts.elastic.co/packages/7.x/apt stable main" | tee /etc/apt/sources.list.d/elastic-7.x.list
      ```

   - **Update the package information:**

      ```bash
      apt-get update
      ```

      
   - **Install Elasticsearch:**

      ```bash
      apt-get install elasticsearch=7.17.6
      ```
   - **Download the configuration file:**

      ```bash
      curl -so /etc/elasticsearch/elasticsearch.yml https://packages.wazuh.com/4.3/tpl/elastic-basic/elasticsearch_all_in_one.yml
      ```

      
   - **Generate Configuration Files**

      ```bash
      ./wazuh-install.sh -i --generate-config-files
      ```
      
   - **List the files to ensure everything has been downloaded and generated correctly:**

      ```bash
      ls
      ```
   - **Install Wazuh Indexer (node-1)**

      ```bash
      ./wazuh-install.sh -i --wazuh-indexer node-1
      ```
      
   - **Start the Wazuh Cluster**
       
      ```bash
      ./wazuh-install.sh -i --start-cluster
      ```

      
   - **Install Wazuh Server (wazuh-1)**

      ```bash
      ./wazuh-install.sh -i --wazuh-server wazuh-1
      ```
   - **Install Wazuh Dashboard**

      ```bash
      ./wazuh-install.sh -i --wazuh-dashboard dashboard
      ```





## Integrating ELK with the Wazuh


   ### Step 2 Elasticsearch Installation and Configuration

   - **Import the Elasticsearch GPG Key**

      ```bash
      curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elastic.gpg
      ```

   - **Add the Elastic Source List**
   
      ```bash
      echo "deb [signed-by=/usr/share/keyrings/elastic.gpg] https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
      ```
   - **Update Package Lists**
   
      ```bash
      sudo apt update
      ```
   - **Install Elasticsearch**
   
      ```bash
      sudo apt install elasticsearch
      ```
   - **Configure Elasticsearch (edit elasticsearch.yml)**
   
      ```bash
      sudo nano /etc/elasticsearch/elasticsearch.yml
      ```

   - **Start and Enable Elasticsearch**
   
      ```bash
      sudo systemctl start elasticsearch
      ```
      ```bash
      sudo systemctl enable elasticsearch
      ```
   - **You can test whether your Elasticsearch service is running by sending an HTTP request:**
   - 
      ```bash
      curl -X GET "localhost:9200"
      ```
     






   ### Step 3 — Kibana Dashboard installation and configuration

