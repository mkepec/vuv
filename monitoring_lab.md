# Lab Manual: Monitoring Setup with Prometheus & Grafana
Cilj: Konfigurirati nadzor za Linux VM, Windows računalo i Cisco preklopnik (Switch) koristeći Prometheus, Grafana i potrebne eksportere.

## 1. Korištenje Ubuntu Desktop VM u VirtualBoxu 
- Prijavite se s korisničkim imenom `ubuntu` i lozinkom `student`
- Pokrenite Terminal i instalirajte ažuriranja
```bash
sudo apt update && sudo apt upgrade -y
````
- Provjerite IP adresu VMa- i uvjerite se kako je to adresa na istoj mreži kao i laboratorijsko računalo
```bash
ip a
```
> IP adresu računala u labu provjerite s naredbom `ipconfig` kroz `cmd`
- 

## 2. Instalacija Docker-a
### 2.1. Instalacija Docker Engine
```bash
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/docker.gpg
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt update && sudo apt install docker-ce docker-ce-cli containerd.io
```

### 2.2. Dodajte korisnika u Docker grupu
```bash
sudo usermod -aG docker $USER
newgrp docker  # Osvježavanje dozvola za grupu
```

**Provjera**
```bash
docker --version  # Treba prikazati verziju Docker-a
docker run hello-world  # Treba ispisati "Hello from Docker!"
```

Ukoliko je Docker uspješno instaliran možete nastaviti s podešavanjem i pokretanjem Prometheusa

## Pokretanje Prometheusa
### 3.1. Kreiranje direktorija
```bash
mkdir ~/prometheus && cd ~/prometheus
mkdir config data
```

### 3.2. Kreirajte `prometheus.yml`
Koristeći VSCode, kreirajte datoteku `config/prometheus.yml` u direktoriju `prometheus`:
> VSCode možete pokrenuti na način da se krot `Terminal` pozocionirate u željeni direktorij s naredbom `cd ~/prometheus` te tamo zadate naredbu `code .`
```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: "prometheus"
    static_configs:
      - targets: ["localhost:9090"]

  - job_name: "node_exporter"
    static_configs:
      - targets: ["localhost:9100"]

  - job_name: "snmp_exporter"
    static_configs:
      - targets: ["localhost:9116"]  # Port za SNMP eksporter

  - job_name: "windows_exporter"
    static_configs:
      - targets: ["<WINDOWS_HOST_IP>:9182"]  # Zamijenite s IP Windows računala
```

### 3.3. Pokrenite Prometheus container 
```bash
docker run -d --name prometheus \
  -p 9090:9090 \
  -v ~/prometheus/config:/etc/prometheus \
  -v ~/prometheus/data:/prometheus \
  prom/prometheus:latest
```

**Provjera:**
- Otvorite **http://<VM_IP>:9090** u pregledniku.
- U **Status > Targets** provjerite je li Prometheus aktivan.


## 4. Pokretanje Grafane
### 4.1. Pokrenite Grafana container
```bash
docker run -d --name grafana \
  -p 3000:3000 \
  grafana/grafana:latest
```

**Provjera:**
- Otvorite **http://<VM_IP>:3000** u pregledniku. Prijavite se s `admin`/`admin`.


## 5. Postavljanje exportera 
### 5.1. Node Exporter (Linux VM) 
```bash
docker run -d --name node_exporter \
  --net="host" \
  -v "/:/host:ro,rslave" \
  quay.io/prometheus/node-exporter:latest \
  --path.rootfs=/host
```

**Provjera:**
- Provjerite `http://<VM_IP>:9100/metrics` za sirove metrike.

### 5.2. SNMP Exporter (Mrežna oprema) 
```bash
docker run -d --name snmp_exporter \
  -p 9116:9116 \
  -v ~/prometheus/snmp.yml:/etc/snmp_exporter/snmp.yml \
  prom/snmp-exporter:latest
```
> Napomena: prije pokretanja je potrebno kreirati `snmp.yml` i urediti ga. 

**Provjera:**
- Provjerite **http://<VM_IP>:9116/snmp?target=<SWITCH_IP>** (zamijenite s IP preklopnika).

### 5.3. Windows Exporter (Windows PC) 
1. Preuzmite windows_exporter.msi s GitHub Releases https://github.com/prometheus-community/windows_exporter.
2. Pokrenite instalaciju s defaultnim postavkama.
3. Dopustite port `9182` u Windows vatrozidu.

**Provjera:**
- Pristupite **http://<WINDOWS_HOST_IP>:9182/metrics** s Ubuntu VM-a


## 6. Ažuriranje Prometheus targeta 
1. Ažurirajte `prometheus.yml` tako što ćete dodati blok za nadoz preklopnika
```yaml
- job_name: "cisco_switch"
  static_configs:
    - targets: ["<SWITCH_IP>"]  # IP Cisco preklopnika
  metrics_path: /snmp
  params:
    module: [if_mib]
  relabel_configs:
    - source_labels: [__address__]
      target_label: __param_target
    - source_labels: [__param_target]
      target_label: instance
    - target_label: __address__
      replacement: <VM_IP>:9116  # Adresa SNMP eksportera
```
2. Ponovno učitajte Prometheus:
```bash
docker exec prometheus kill -SIGHUP 1
```

**Provjera:**
- U **Status > Targets** u Prometheusu svi targeti trebaju biti UP.


## 7. Postavljanje Grafana nadzornih ploča (Dashboards) 
### 7.1. Dodajte Prometheus kao izvor podataka 
1. U Grafana sučelju, idite na Configuration > Data Sources > Add data source.
2. Odaberite Prometheus, postavite URL na http://<VM_IP>:9090.

### 7.2. Uvoz nadzornih ploča 
Dostupne nadzorne ploče (Dashboards) možete pretražiti na stranici https://grafana.com/grafana/dashboards/ 
1. Node Exporter dashboard:
Idite na Create > Import > Dashboard ID: 1860.
2. Windows Exporter dashboard:
Uvezite ID: 10467.
3. SNMP ploča:
Uvezite ID: 14405 i prilagodite metriku za Cisco prekidač.


## 8. SNMP nadzor za Cisco preklopnik
1. Omogućite SNMP na Cisco prekidaču (konfigurirajte community string).
2. Ažurirajte snmp.yml s OID-ima za prekidač (npr. statike sučelja).
3. Ponovno pokrenite SNMP eksporter:
```bash
docker restart snmp_exporter
```

**Provjera:**
- Provjerite metriku SNMP eksportera u Prometheusu

