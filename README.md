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

### Step 3: Certificates Creation

 - **Download configuration file for creating certificates:**

      ```bash
      curl -so /usr/share/elasticsearch/instances.yml https://packages.wazuh.com/4.3/tpl/elastic-basic/instances_aio.yml
      ```

 - **Create certificates using the elasticsearch-certutil tool:**
      ```bash
      /usr/share/elasticsearch/bin/elasticsearch-certutil cert ca --pem --in instances.yml --keep-ca-key --out ~/certs.zip
      ```


 - **Extract the generated certificates:**
      ```bash
      unzip ~/certs.zip -d ~/certs
      ```

 - **Copy certificates to the Elasticsearch directory:**
      ```bash
      mkdir /etc/elasticsearch/certs/ca -p
      cp -R ~/certs/ca/ ~/certs/elasticsearch/* /etc/elasticsearch/certs/
      chown -R elasticsearch: /etc/elasticsearch/certs
      chmod -R 500 /etc/elasticsearch/certs
      chmod 400 /etc/elasticsearch/certs/ca/ca.* /etc/elasticsearch/certs/elasticsearch.*
      rm -rf ~/certs/ ~/certs.zip
      ```


 - **Enable and start the Elasticsearch service:**
      ```bash
      systemctl daemon-reload
      systemctl enable elasticsearch
      systemctl start elasticsearch
      ```


 - **Generate credentials for pre-built roles and users:**
      ```bash
      /usr/share/elasticsearch/bin/elasticsearch-setup-passwords auto
      ```

<details>
   <summary>Output</summary>   

```bash
- Changed password for user apm_system  
  PASSWORD apm_system = lLPZhZkB6oUOzzCrkLSF

- Changed password for user kibana_system  
  PASSWORD kibana_system = TaLqVOnSoqKTYLIU0vDn

- Changed password for user kibana  
  PASSWORD kibana = TaLqVOvXoqKTYLIU0vDn

- Changed password for user logstash_system  
  PASSWORD logstash_system = UtuDv2tWkXGYL83v9kWA

- Changed password for user beats_system  
  PASSWORD beats_system = qZcbvCslafMpoEOrE9Ob

- Changed password for user remote_monitoring_user  
  PASSWORD remote_monitoring_user = LzJpQiSylncmCU2GLBTS

- Changed password for user elastic  
  PASSWORD elastic = AN4UeQGA7HGl5iHpMla7
```

