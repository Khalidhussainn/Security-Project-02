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

    

