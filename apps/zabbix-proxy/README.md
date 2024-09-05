# docker
zabbix proxy setup

## Tls key:
Du må lage en key og sette tls identity i .env fila
Disse må matche på serveren 

```bash
mkdir enc
openssl rand -hex 32 > enc/psk
```
