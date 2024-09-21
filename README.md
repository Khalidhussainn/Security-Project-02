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
            Changed password for user apm_system  
                    PASSWORD apm_system = lLPZhZkB6oUOzzCrkLSF
                  
            Changed password for user kibana_system  
                    PASSWORD kibana_system = TaLqVOnSoqKTYLIU0vDn
                  
            Changed password for user kibana  
                    PASSWORD kibana = TaLqVOvXoqKTYLIU0vDn
                  
            Changed password for user logstash_system  
                    PASSWORD logstash_system = UtuDv2tWkXGYL83v9kWA
                  
            Changed password for user beats_system  
                    PASSWORD beats_system = qZcbvCslafMpoEOrE9Ob
                  
            Changed password for user remote_monitoring_user  
                    PASSWORD remote_monitoring_user = LzJpQiSylncmCU2GLBTS
                  
            Changed password for user elastic  
                    PASSWORD elastic = AN4UeQGA7HGl5iHpMla7
      ```
      </details>

 - **To check that the installation was made successfully:**
      ```bash
      curl -XGET https://localhost:9200 -u elastic:<elastic_password> -k
      ```

      <details>
         <summary>Output</summary>   
      
      ```bash
            {
              "name" : "elasticsearch",
              "cluster_name" : "elasticsearch",
              "cluster_uuid" : "BgdIyCXxSPGeRusvb6-_Qw",
              "version" : {
                "number" : "7.17.6",
                "build_flavor" : "default",
                "build_type" : "rpm",
                "build_hash" : "f65e9d338dc1d07b642e14a27f338990148ee5b6",
                "build_date" : "2022-08-23T11:08:48.893373482Z",
                "build_snapshot" : false,
                "lucene_version" : "8.11.1",
                "minimum_wire_compatibility_version" : "6.8.0",
                "minimum_index_compatibility_version" : "6.0.0-beta1"
              },
              "tagline" : "You Know, for Search"
            }
      ```
      </details>


 




### Step 4: Wazuh Installation

 - **Add the Wazuh repository:**

      ```bash
      curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | gpg --no-default-keyring --keyring gnupg-ring:/usr/share/keyrings/wazuh.gpg --import && chmod 644       /usr/share/keyrings/wazuh.gpg
      ```

 - **Add the Wazuh repository to the sources list:**
      ```bash
      echo "deb [signed-by=/usr/share/keyrings/wazuh.gpg] https://packages.wazuh.com/4.x/apt/ stable main" | tee -a /etc/apt/sources.list.d/wazuh.list 
      ```


 - **Update the package information:**
      ```bash
      apt-get update
      ```
 - **Install the Wazuh manager:**
      ```bash
      apt-get install wazuh-manager=4.3.11-1
      ```
      
 - **Enable and start the Wazuh manager service:**
      ```bash
      systemctl daemon-reload
      systemctl enable wazuh-manager
      systemctl start wazuh-manager
      ```
 - **Run the following command to check if the Wazuh manager is active:**
      ```bash
      systemctl status wazuh-manager
      ```

### Step 5: Filebeat Installation

 - **Install the Filebeat package:**
      ```bash
      apt-get install filebeat=7.17.6
      ```



 - **Download pre-configured Filebeat config file:**
      ```bash
      curl -so /etc/filebeat/filebeat.yml https://packages.wazuh.com/4.3/tpl/elastic-basic/filebeat_all_in_one.yml
      ```



 - **Download the alerts template for Elasticsearch:**

      ```bash
      curl -so /etc/filebeat/wazuh-template.json https://raw.githubusercontent.com/wazuh/wazuh/4.3/extensions/elasticsearch/wazuh-template.json
      chmod go+r /etc/filebeat/wazuh-template.json
      ```



 - **Download the Wazuh module for Filebeat:**
      ```bash
      curl -s https://packages.wazuh.com/4.x/filebeat/wazuh-filebeat-0.2.tar.gz | tar -xvz -C /usr/share/filebeat/module
      ```

 - **Edit the Filebeat configuration file to add Elasticsearch password:**
      ```bash
      nano /etc/filebeat/filebeat.yml
      ```

      <details>
         <summary>Output</summary>   
      
      ```bash
      .
      .
      output.elasticsearch.password: <elasticsearch_password>
      .
      .
      ```
      </details>

 - **Copy the certificates into /etc/filebeat/certs/**
      ```bash
      cp -r /etc/elasticsearch/certs/ca/ /etc/filebeat/certs/
      cp /etc/elasticsearch/certs/elasticsearch.crt /etc/filebeat/certs/filebeat.crt
      cp /etc/elasticsearch/certs/elasticsearch.key /etc/filebeat/certs/filebeat.key
      ```

 - **Enable and start the Filebeat service:**
      ```bash
      systemctl daemon-reload
      systemctl enable filebeat
      systemctl start filebeat
      ```
 - **To ensure that Filebeat has been successfully installed**
      ```bash
      filebeat test output
      ```
      <details>
         <summary>Output</summary>   
      
      ```bash
       elasticsearch: https://127.0.0.1:9200...
         parse url... OK
         connection...
           parse host... OK
           dns lookup... OK
           addresses: 127.0.0.1
           dial up... OK
         TLS...
           security: server's certificate chain verification is enabled
           handshake... OK
           TLS version: TLSv1.3
           dial up... OK
         talk to server... OK
         version: 7.17.6
      ```
      </details>

    
### Step 6: Kibana Installation
 - **Install the Kibana package:**
      ```bash
      apt-get install kibana=7.17.6
      ```

 - **Copy Elasticsearch certificates to Kibana:**

      ```bash
      mkdir /etc/kibana/certs/ca -p
      cp -R /etc/elasticsearch/certs/ca/ /etc/kibana/certs/
      cp /etc/elasticsearch/certs/elasticsearch.key /etc/kibana/certs/kibana.key
      cp /etc/elasticsearch/certs/elasticsearch.crt /etc/kibana/certs/kibana.crt
      chown -R kibana:kibana /etc/kibana/
      chmod -R 500 /etc/kibana/certs
      chmod 440 /etc/kibana/certs/ca/ca.* /etc/kibana/certs/kibana.*

      ```
 - **Download the Kibana configuration file:**
      ```bash
      curl -so /etc/kibana/kibana.yml https://packages.wazuh.com/4.3/tpl/elastic-basic/kibana_all_in_one.yml
      ```


 - **Edit the kibana file to add the Password:**
      ```bash
      nano /etc/kibana/kibana.yml
      ```

      <details>
         <summary>Output</summary>   
      
      ```bash
      .
      .
      elasticsearch.password: <elasticsearch_password>
      .
      .
      ```
      </details>

      
 - **Create Kibana data directory: /usr/share/kibana/data**
   
      ```bash
      mkdir /usr/share/kibana/data
      chown -R kibana:kibana /usr/share/kibana
      ```
 - **Install the Wazuh Kibana plugin:**
      ```bash
      cd /usr/share/kibana
      sudo -u kibana /usr/share/kibana/bin/kibana-plugin install https://packages.wazuh.com/4.x/ui/kibana/wazuh_kibana-4.3.11_7.17.6-1.zip
      ```
      
 - **Link Kibana's socket to privileged port 443:**

      ```bash
      setcap 'cap_net_bind_service=+ep' /usr/share/kibana/node/bin/node
      ```
      
 - **Enable and start the Kibana service:**
      ```bash
      systemctl daemon-reload
      systemctl enable kibana
      systemctl start kibana
      ```
