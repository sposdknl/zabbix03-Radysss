# Install Zabbix Agent2 on Ubuntu
Repositories for teaching purposes at SPOS DK

![Ubuntu and ZabbixAgent2 OSY AI](../Images/osy-Ubuntu-ZabbixAgent2.webp)

Repository pro vyuku na SPOS DK

## Automatická instalace Zabbix Agent2 na OS Linux Ubuntu

- Vagrantfile obsahuje sekci pro aplikaci příkazů pro instalaci monitorovacího
[Zabbix Agent2](https://www.zabbix.com/).

### Instalace Zabbix Agent2

```console
wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest+ubuntu22.04_all.deb
dpkg -i zabbix-release_latest+ubuntu22.04_all.deb

apt-get update
apt-get install -y zabbix-agent2 zabbix-agent2-plugin-*

systemctl enable zabbix-agent2
systemctl start zabbix-agent2
```

### Konfigurace Zabbix Agent2

```console
joe /etc/zabbix/zabbix_agent2.conf
...
Hostname=ubuntu-8e714c18
Server=enceladus.pfsense.cz
ServerActive=enceladus.pfsense.cz
Timeout=30
HostMetadata=SPOS

systemctl restart zabbix-agent2
```
...

### Úpravy v config
UNIQUE_HOSTNAME="ubuntu-$(uuidgen)"
SHORT_HOSTNAME=$(echo $UNIQUE_HOSTNAME | cut -d'-' -f1,2),
...
cat <<EOF | sudo tee -a /etc/zabbix/zabbix_agent2.conf

Hostname=$SHORT_HOSTNAME
Server=192.168.1.2
ServerActive=192.168.1.2
HostMetadata=SPOS
Timeout=30
EOF

### Upravy v Vagrantfile
# Interní síť (Zabbix appliance síť)
        ubuntu.vm.network "private_network", ip: "192.168.1.3", virtualbox__intnet: true