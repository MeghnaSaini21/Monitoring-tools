# Monitoring-tools
--> Prometheus:
Install Prometheus and set up on Ubuntu instance
First we have to create a Linux user for Prometheus and download Prometheus:
  sudo useradd --system --no-create-home --shell /bin/false prometheus
  wget https://github.com/prometheus/prometheus/releases/download/v2.47.1/prometheus-2.47.1.linux-amd64.tar.gz
  tar -xvf prometheus-2.47.1.linux-amd64.tar.gz
  tar -xvf prometheus-2.47.1.linux-amd64.tar.gz
Then, we have to move them and create directories 
  sudo mkdir -p /data /etc/prometheus
  sudo mv prometheus promtool /usr/local/bin/
  sudo mv consoles/ console_libraries/ /etc/prometheus/
  sudo mv prometheus.yml /etc/prometheus/prometheus.yml
Setup the ownership for prometheus user:
  sudo chown -R prometheus:prometheus /etc/prometheus/ /data/
setup the systemd configuration:
sudo vim /etc/systemd/system/prometheus.service
{#this is written in the configuration file
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

StartLimitIntervalSec=500
StartLimitBurst=5

[Service]
User=prometheus
Group=prometheus
Type=simple
Restart=on-failure
RestartSec=5s
ExecStart=/usr/local/bin/prometheus \
  --config.file=/etc/prometheus/prometheus.yml \
  --storage.tsdb.path=/data \
  --web.console.templates=/etc/prometheus/consoles \
  --web.console.libraries=/etc/prometheus/console_libraries \
  --web.listen-address=0.0.0.0:9090 \
  --web.enable-lifecycle

[Install]
WantedBy=multi-user.target

At last, start the prometheus:
  sudo systemctl enable prometheus
  sudo systemctl start prometheus
  sudo systemctl status prometheus
![Screenshot 2025-06-10 225416](https://github.com/user-attachments/assets/66a07cad-12d0-472c-b3dc-c1b2369634d5)

Go to the browser and checks the status : http://<your-server-ip>:9090
As after setup the NodeExporter and BlackboxExporter as shown in the targets :
 ![WhatsApp Image 2025-06-11 at 10 11 08_d5a540d2](https://github.com/user-attachments/assets/19058041-5f80-4eb2-9fb6-902954baebd7)
 
--> Grafana: 
Install Grafana and set up on Ubuntu instance
Step 1: Install dependencies and key set-up GPG key
  sudo apt-get update
  sudo apt-get install -y apt-transport-https software-properties-common
  wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
Step 2:Add Repo for grafana and update and install grafana
  echo "deb https://packages.grafana.com/oss/deb stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
  sudo apt-get update
  sudo apt-get -y install grafana
Step 3: start the service
  sudo systemctl enable grafana-server
  sudo systemctl start grafana-server
  sudo systemctl status grafana-server
![Screenshot 2025-06-11 173321](https://github.com/user-attachments/assets/995e43c6-fc07-4a62-b0f6-35fca54643c8)

Check on the browser for dashboard of Grafana: "http://IP:3000
![Screenshot 2025-06-11 173837](https://github.com/user-attachments/assets/af0489a2-247a-4dae-b955-f5a009eb41b9)

Then follow this process for various Dashboards are as follows:
![Screenshot 2025-06-11 190044](https://github.com/user-attachments/assets/5888f087-52f2-4f82-aeaf-ad946c7646d0)

