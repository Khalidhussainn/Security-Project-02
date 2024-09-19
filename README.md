# Step-by-Step Wazuh Installation Guide

*** 1. Create a Directory for Wazuh ***
```bash
mkdir wazuh
cd wazuh
```

#### 2. Download the Wazuh Installation Script

```bash
wget https://packages.wazuh.com/4.9/wazuh-install.sh
```
#### 3. Download config.yml file with the following command:

```bash
wget https://packages.wazuh.com/4.9/config.yml
```

#### 4. Switch to Root User

```bash
sudo su
```
#### 5. Make the Installation Script Executable

```bash
chmod +x wazuh-install.sh
```
#### 6. Edit the Configuration File

```bash
nano config.yml
```
#### 7. Generate Configuration Files

```bash
./wazuh-install.sh -i --generate-config-files
```
#### 8. List the files to ensure everything has been downloaded and generated correctly:

```bash
ls
```
#### 9. Install Wazuh Indexer (node-1)

```bash
./wazuh-install.sh -i --wazuh-indexer node-1
```
#### 10. Start the Wazuh Cluster
```bash
./wazuh-install.sh -i --start-cluster
```
#### 11. Install Wazuh Server (wazuh-1)

```bash
./wazuh-install.sh -i --wazuh-server wazuh-1
```
#### 12. Install Wazuh Dashboard
```bash
./wazuh-install.sh -i --wazuh-dashboard dashboard
```





# Testing



# Roadmap for Setting Up Wazuh with the ELK Stack

## Step 1: Install Wazuh Manager

1. **Prepare the System: Update Your Package Lists**

    ```bash
    sudo apt update
    ```

2. **Install Wazuh Repository**

    ```bash
    curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | sudo apt-key add -
    echo "deb https://packages.wazuh.com/4.x/apt/ stable main" | sudo tee /etc/apt/sources.list.d/wazuh.list
    sudo apt update
    ```

3. **Install Wazuh Manager**

    ```bash
    sudo apt install wazuh-manager
    ```

## Step 2: Install Wazuh Agent (on Endpoints)

1. **Add Wazuh Repository and Install the Wazuh Agent**

    ```bash
    sudo apt install wazuh-agent
    ```

2. **Configure the Wazuh Agent**

    Edit the agent’s configuration file `/var/ossec/etc/ossec.conf` and replace `<MANAGER_IP>` with your Wazuh Manager’s IP.

    ```xml
    <server>
       <address>MANAGER_IP</address>
    </server>
    ```

3. **Start the Wazuh Agent**

    ```bash
    sudo systemctl start wazuh-agent
    ```

## Step 3: Install the ELK Stack

1. **Install Elasticsearch**

    - **Download and Install the Elasticsearch Public Signing Key**

      ```bash
      wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
      ```

    - **Add Elasticsearch Repository**

      ```bash
      echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
      sudo apt update
      ```

    - **Install Elasticsearch**

      ```bash
      sudo apt install elasticsearch
      ```

2. **Install Logstash**

    ```bash
    sudo apt install logstash
    ```

3. **Install Kibana**

    ```bash
    sudo apt install kibana
    ```

## Step 4: Configure Wazuh with ELK

1. **Integrate Wazuh with Elasticsearch**

    ```bash
    sudo apt install wazuh-elastic7
    ```

2. **Configure Logstash for Wazuh**

    - **Install the Wazuh Logstash Module**

      ```bash
      sudo apt install wazuh-logstash
      ```

    - **Configure Logstash**

      Edit the Logstash configuration file to set up the input, filter, and output plugins for Wazuh.

3. **Configure Kibana for Wazuh**

    ```bash
    sudo apt install wazuh-kibana
    ```

4. **Start and Enable Services**

    ```bash
    sudo systemctl enable elasticsearch
       ```
     ```bash
    sudo systemctl enable kibana
       ```
     ```bash
    sudo systemctl enable logstash
       ```
        ```bash
    sudo systemctl start elasticsearch
       ```
     ```bash
    sudo systemctl start kibana
       ```
     ```bash
    sudo systemctl start logstash
    ```

## Step 5: Access the Wazuh Dashboard

1. **Open Your Web Browser and Access Kibana (Wazuh’s Dashboard)**

    ```plaintext
    http://<KIBANA_IP>:5601
    ```

2. **Use the Kibana Interface**

    Visualize Wazuh alerts and monitor your environment.
