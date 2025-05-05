# Monitor
Some interesting text

- Opslag van sensor data met `InfluxDB` database
- Visualisatie van de data met `Grafana` dashboard

## Installatie
Voor we beginnen zorgen we dat ons systeem up-to-date is:
```sh
sudo apt update
sudo apt upgrade -y
```
### InfluxDB3 Core

Installeer `InfluxDB`, en volg de instructies van het installatiescript
```sh
curl -O https://www.influxdata.com/d/install_influxdb3.sh
sh install_influxdb3.sh
```
Om de `InfluxDB CLI` te gebruiken, voor het volgende commando uit:
```sh
source '/home/mechatronica/.bashrc'
```

>[WARNING]
> Dit is de token, hou deze goed bij!
> Token: apiv3_VRKtRtzOEYJJ-jDKN8uV3iBuh3g93ArpBdnH_GPyJxAZ0CF_pNDk-nlOxRdzWd4oATd7zppYf2Aees8wP3wScg

#### referenties:
https://docs.influxdata.com/influxdb3/core/reference/cli/influxdb3/

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

#### Configure Grafana auto-login
in `/etc/grafana/grafana.ini`, wijzig deze lijn naar 'true':
```
[auth]
disable_login_form = true
```
### Chromium (browser)
TODO: ... Grafana werkt met een web-interface ...

Installeer de `Chromium` web browser:
```
sudo apt install chromium-browser
```
Om te zorgen dat de `Chromium` browser start op ons `Grafana` dashboard wanneer de Raspberry Pi opstart, voeg volgende lijn toe in het bestand `/etc/xdg/labwc/autostart`:
```
sleep 5 && /usr/bin/chromium-browser --kiosk --noerrdialogs --disable-infobars --ozone-platform=wayland http://localhost:3000/d/dasboard-unique-id/monitor?orgId=1&kiosk= &
```


#### Configureer `InfluxDB` als databron voor `Grafana`
1) In de browser, open het Grafana dashboard `https://localhost:3000`, en log in met de `admin` credentials.
2) 