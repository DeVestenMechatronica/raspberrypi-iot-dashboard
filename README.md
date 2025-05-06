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
#### Installatie
Installeer `InfluxDB`, en volg de instructies van het installatiescript
```sh
curl -O https://www.influxdata.com/d/install_influxdb3.sh
sh install_influxdb3.sh
```
Om de `InfluxDB CLI` te kunnen gebruiken, moet je eerst het volgende commando uitvoeren. Test of je het kan gebruiken door de versie op te vragen.
```sh
source ~/.bashrc
influxdb3 --version
```

Database server starten:
```sh
influxdb3 serve --object-store file --data-dir ~/.influxdb3 --node-id node0
```

#### Aanmaken database & gebruiker
In de commandline:
```sh
influxdb3 
```
Toegang `token` genereren voor de `admin` gebruiker:
```sh
influxdb3 create token --admin
```
>[!WARNING]
> **Hou deze `token` goed bij!** Deze hebben we nodig om toegang the hebben tot de database.
> Token: apiv3_twVOSg3DPHEVzmaQ64sbDv6tlCdo9jFC8JG0UQgHHxfDaWNUYnauYpZXpLLm7QtOi2YSITPkL5dZy760HfeKAg

Onze `database` voor de sensor data aanmaken:
```sh
influxdb3 create database sensors --token apiv3_twVOSg3DPHEVzmaQ64sbDv6tlCdo9jFC8JG0UQgHHxfDaWNUYnauYpZXpLLm7QtOi2YSITPkL5dZy760HfeKAg
```

#### referenties:
https://docs.influxdata.com/influxdb3/core/reference/cli/influxdb3/



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

[auth.anonymous]
# enable anonymous access
enabled = true
```
### Chromium (browser)
TODO: ... Grafana werkt met een web-interface ...

Installeer de `Chromium` web browser:
```
sudo apt install chromium-browser
```
Om te zorgen dat de `Chromium` browser start op ons `Grafana` dashboard wanneer de Raspberry Pi opstart, voeg volgende lijn toe in het bestand `/etc/xdg/labwc/autostart`:
```
sleep 5 && /usr/bin/chromium-browser --kiosk --noerrdialogs --disable-infobars --ozone-platform=wayland http://localhost:3000/d/bel1pwxub2fwga/monitor?orgId=1&kiosk= &

```

>[!TIP]
>Om de `Chromium` browser te sluiten wanneer die in kiosk-modus is, sluit je een toetsenbord aan en druk je `alt+F4`


#### Configureer `InfluxDB` als databron voor `Grafana`
1) In de browser, open het Grafana dashboard `https://localhost:3000`, en log in met de `admin` credentials.
2) 