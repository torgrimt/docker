# docker
zabbix proxy setup

## Tls key:
Du må lage en key og sette tls identity i .env fila
Disse må matche på serveren 

Søppel image, så man må gjøre en h0ax på db_data

```bash
mkdir db_data
sudo chown 1997:1995 db_data

mkdir enc
openssl rand -hex 32 > enc/psk
```
