# Monitor
Some interesting text

- Opslag van sensor data met InfluxDB
- Visualisatie van de data met Grafana

## Installatie
Voor we beginnen zorgen we dat ons systeem up-to-date is:
```sh
sudo apt update
sudo apt upgrade -y
```
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
Controleer de status van de `InfluxDB` service
```sh
sudo systemctl status influxdb
```
#### Aanmaken database & gebruiker
In de commandline:
```sh
influx
> CREATE DATABASE home
> CREATE USER grafana WITH PASSWORD 'mechatronica' WITH ALL PRIVILEGES
> GRANT ALL PRIVILEGES ON home TO grafana
> exit
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
Controleer de status van de `Grafana` service
```sh
sudo systemctl status grafana-server
```

#### Configureer `InfluxDB` als databron voor `Grafana`
1) In de browser, open het Grafana dashboard `https://localhost:3000`, en log in met de `admin` credentials.
2) 