# Monitor
Some interesting text

- Opslag van sensor data met InfluxDB
- Visualisatie van de data met Grafana

## Installatie
### InfluxDB

Voeg de `InfluxDB` repository en key toe aan de `apt` package manager (als administrator):
```sh
sudo curl -fsSL https://repos.influxdata.com/influxdb.key | gpg --dearmor | sudo tee /etc/apt/keyrings/influxdata-archive-keyring.gpg > /dev/null
sudo echo "deb [signed-by=/etc/apt/keyrings/influxdata-archive-keyring.gpg] https://repos.influxdata.com/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/influxdb.list
```
Installeer `InfluxDB` met `apt` (als administrator):
```sh
sudo apt update
sudo apt install influxdb
```
Start de `InfluxDB` systeem service met `systemctl`:
```sh
sudo systemctl unmask influxdb
sudo systemctl enable influxdb
sudo systemctl start influxdb
```
### Grafana

Voeg de `Grafana` repository en key to aan de `apt` package manager (als administrator):
```sh
sudo curl -fsSL https://packages.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana-archive-keyring.gpg > /dev/null
sudo echo "deb [signed-by=/etc/apt/keyrings/grafana-archive-keyring.gpg] https://packages.grafana.com/oss/deb stable main" | sudo tee /etc/apt/sources.list.d/grafana.list
```
Installeer `Grafana` met `apt` (als administrator):
```sh
sudo apt update
sudo apt install grafana
```
Start de `Grafana` systeem service met `systemctl`:
```sh
sudo systemctl enable grafana-server
sudo systemctl start grafana-server
```