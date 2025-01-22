# Prometheus & Grafana Lab 

## Prerequisistes

### Zaustavite prethodno instalirane servise
```
systemctl stop prometheus
systemctl stop grafana
```

### Install Docker

Update sistema i instalacija potrebnih paketa
```
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```

Dodavanje Docker GPG ključa
```
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

Dodavanje Docker repozitorija
```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Update paketa
```
sudo apt update
```

Instalacija Docker enginea
```
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Provjera Docker instalacije
```
sudo docker run hello-world
```

Upravljanje dockerom kao non-root korisnik
```
sudo usermod -aG docker ${USER}
```
nakon ove naredbe odjavite se iz terminala i ponovno ga pokrenite




### Folder struktura

Kreirajte potrebnu folder strukturu

```
mkdir -p ~/monitoring-lab/prometheus ~/monitoring-lab/snmp-exporter
cd ~/monitoring-lab
```


