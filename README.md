# Step-by-Step Wazuh Installation and integrating with ELK Stack

### Step 1 Wazuh Installation

   - **Create a Directory for Wazuh**
      ```bash
      mkdir wazuh
      cd wazuh
      ```

   - **Download the Wazuh Installation Script**

      ```bash
      wget https://packages.wazuh.com/4.9/wazuh-install.sh
      ```
   - **Download config.yml file with the following command:**
   
      ```bash
      wget https://packages.wazuh.com/4.9/config.yml
      ```

   - **Switch to Root User**

      ```bash
      sudo su
      ```
   - **Make the Installation Script Executable**

      ```bash
      chmod +x wazuh-install.sh
      ```
   - **Edit the Configuration File**

      ```bash
      nano config.yml
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

