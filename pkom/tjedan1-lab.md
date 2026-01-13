# Laboratorijski vodič - 1. tjedan: Operation Dark Network
## Uspostavljanje vidljivosti - Prikupljanje podataka i infrastruktura za nadzor

**Kolegij:** Projektiranje komunikacijskih mreža
**Trajanje vježbe:** 1. tjedan od 3
**Procijenjeno vrijeme:** 4-6 sati (2 laboratorijske sesije)
**Ishodi učenja:** LO6, LO8

---

## Sadržaj

1. [Pregled i ciljevi](#pregled-i-ciljevi)
2. [Preduvjeti i okruženje](#preduvjeti-i-okruženje)
3. [Misija 0: Pristup i izviđanje](#misija-0-pristup-i-izviđanje)
4. [Misija 1: Postavljanje sustava za nadzor](#misija-1-postavljanje-sustava-za-nadzor)
5. [Misija 2: Otkrivanje mreže](#misija-2-otkrivanje-mreže)
6. [Misija 3: Konfiguracija SNMP nadzora](#misija-3-konfiguracija-snmp-nadzora)
7. [Misija 4: Nadzor vlastitog servera](#misija-4-nadzor-vlastitog-servera)
8. [Misija 5: Provjera i dokumentacija](#misija-5-provjera-i-dokumentacija)
9. [Vodič za rješavanje problema](#vodič-za-rješavanje-problema)
10. [Uvjeti za predaju](#uvjeti-za-predaju)

---

## Pregled i ciljevi

### Što gradite

Do kraja ovog tjedna, postavit ćete **modernu infrastrukturu za nadzor mreže** koristeći:
- **Prometheus** - Baza podataka vremenskih serija koja prikuplja metrike
- **SNMP Exporter** - Most između Prometheusa i mrežnog hardwarea
- **Node Exporter** - Prikuplja metrike s vašeg Linux servera

### Ciljevi učenja

Nakon završetka ove vježbe, moći ćete:
- ✅ Objasniti arhitekturu sustava za nadzor temeljenih na "pull" modelu
- ✅ Postaviti kontejneriziranu infrastrukturu koristeći Docker Compose
- ✅ Razumjeti osnove SNMP-a (OID-ovi, MIB-ovi, Community Strings)
- ✅ Konfigurirati Prometheus za prikupljanje podataka s više vrsta ciljeva
- ✅ Otklanjati probleme mrežne povezivosti i SNMP pristupa
- ✅ Provjeriti prikupljanje podataka nadzora

### Velika slika

```
┌─────────────────────────────────────────────────────────┐
│                    VAŠ MONITORING VM                    │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  │
│  │  Prometheus  │  │SNMP Exporter │  │Node Exporter │  │
│  │(Port 9090)   │  │(Port 9116)   │  │(Port 9100)   │  │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘  │
│         │                  │                  │          │
│         └─────────┬────────┘──────────────────┘          │
│                   │                                      │
└───────────────────┼──────────────────────────────────────┘
                    │
                    │ Prikuplja metrike
                    ▼
    ┌───────────────────────────────────────┐
    │        MREŽNI HARDWARE                │
    │  • MikroTik RB3011 (Router)           │
    │  • MikroTik CSS326 (Switch)           │
    │  • Cisco Catalyst 2950 (Switch)       │
    └───────────────────────────────────────┘
```

---

## Preduvjeti i okruženje

### Što ste dobili

Iz "prenosa posla na salveti", imate:
- **IP servera:** `192.168.3.?` (Vaš Linux VM)
- **Prijava:** `lab` / `password`
- **SNMP Community String:** `Public_vtx`

### Što trebate na svom računalu

- SSH klijent (PuTTY na Windowsima, ili ugrađeni terminal na macOS/Linux)
- Uređivač teksta za dokumentiranje rada
- Web preglednik (za pristup Prometheusu)

### Potreban softver na VM-u

Sljedeće bi već trebalo biti instalirano na vašem VM-u. Ako nije, instalirat ćete ih u Misiji 0:
- Docker
- Docker Compose
- Osnovni alati za mrežu (nmap, snmpwalk)

---

## Misija 0: Pristup i izviđanje

**Cilj:** Pristupiti serveru za nadzor i provjeriti njegovo zdravlje.

### Korak 0.1: Početno povezivanje

```bash
# S vašeg računala, spojite se SSH-om na server
ssh lab@192.168.3.x
# Password: password
```

**✅ Kontrolna točka:** Trebali biste vidjeti naredbeni redak. Zabilježite hostname i verziju OS-a.

### Korak 0.2: Provjera zdravlja sustava

```bash
# Provjerite verziju OS-a
lsb_release -a

# Provjerite dostupan prostor na disku (trebate najmanje 5GB slobodno)
df -h

# Provjerite memoriju (trebate najmanje 2GB slobodno)
free -h

# Provjerite Docker instalaciju
docker --version
docker-compose --version
```

**✅ Kontrolna točka:** Docker i Docker Compose bi trebali biti instalirani. Ako nisu, instalirajte ih:


Uklonite konfliktne pakete
```bash
sudo apt remove $(dpkg --get-selections docker.io docker-compose docker-compose-v2 docker-doc podman-docker containerd runc | cut -f1)
```

Podesite Docker `apt` repozitorij

```bash
# Dodajte Dockerov službeni GPG ključ:
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Dodajte repozitorij u Apt izvore:
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update
```

Instalirajte Docker pakete
```bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Docker servis se treba automatski pokrenuti nakon instalacije, ako želite možete provjeriti je li Docker pokrenut s ovom naredbom:
```bash
sudo systemctl status docker
```

Ako je potrebno ručno pokrenuti Docker, to možete napraviti s ovom naredbom:
```bash
sudo systemctl start docker
```

Potvrdite da je instalacija bila uspješna tako što ćete pokrenuti `hello-world` image:
```bash
sudo docker run hello-world
```
Ova naredba će preuzeti testni image i pokrenuti ga u containeru. kada se pokrene, ispisat će potvrdnu poruku i zaustaviti container.

Kako bi omogućili pokretanje Dockera bez sudo naredbi napravite slijedeće:

Kreirajte `docker` grupu
```bash
sudo groupadd docker
```

Dodajte svog korisnika `lab` u `docker` grupu
```bash
sudo usermod -aG docker $USER
```

Odjavite se i ponovno se prijavite kako bi se primijenila pripadnost grupi. A možete i pokrenuti ovu naredbu:
```bash
newgrp docker
```

Potvrdite da možete pokrenuti `docker` naredbe bez `sudo`.
```bash
docker run hello-world
```

Podesite da se Docker starta kod pokretanja sustava
```bash
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```

### Korak 0.3: Mrežno izviđanje

```bash
# Provjerite mrežna sučelja vašeg VM-a
ip addr show

# Provjerite povezivost s lab mrežom
ping -c 4 8.8.8.8

# Instalirajte alate za skeniranje mreže
sudo apt update
sudo apt install -y nmap snmp snmp-mibs-downloader
```

**✅ Kontrolna točka:** Vaš VM bi trebao imati:
- Barem jedno mrežno sučelje s IP adresom
- Internet povezivost (za preuzimanje Docker slika)
- Instalirane mrežne alate

### Korak 0.4: Stvaranje radnog direktorija

```bash
# Stvorite direktorij za vaš monitoring stack
mkdir -p /home/lab/monitoring
cd /home/lab/monitoring
```

**📝 Dokumentirajte:** Zabilježite IP adresu vašeg VM-a, naziv sučelja i gateway. Trebat će vam ove informacije kasnije.

---

## Misija 1: Postavljanje sustava za nadzor

**Cilj:** Postaviti Prometheus i SNMP Exporter koristeći Docker Compose.

### Korak 1.1: Stvaranje Docker Compose datoteke

Stvorite glavnu konfiguracijsku datoteku:

```bash
cd /home/lab/monitoring
nano docker-compose.yml
```

**Zalijepite sljedeću konfiguraciju:**

```yaml
version: '3.8'

services:
  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    restart: unless-stopped
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
      - prometheus_data:/prometheus
    command:
      - '--config.file=/etc/prometheus/prometheus.yml'
      - '--storage.tsdb.path=/prometheus'
      - '--storage.tsdb.retention.time=30d'
    networks:
      - monitoring

  snmp_exporter:
    image: prom/snmp-exporter:latest
    container_name: snmp_exporter
    restart: unless-stopped
    ports:
      - "9116:9116"
    volumes:
      - ./snmp.yml:/etc/snmp_exporter/snmp.yml
    networks:
      - monitoring

  node_exporter:
    image: prom/node-exporter:latest
    container_name: node_exporter
    restart: unless-stopped
    ports:
      - "9100:9100"
    command:
      - '--path.rootfs=/host'
    volumes:
      - '/:/host:ro,rslave'
    networks:
      - monitoring

networks:
  monitoring:
    driver: bridge

volumes:
  prometheus_data:
```

**Spremite i izađite:** `Ctrl+X`, zatim `Y`, zatim `Enter`

### Korak 1.2: Stvaranje Prometheus konfiguracije (minimalna)

Za sada, stvorite minimalnu Prometheus konfiguraciju:

```bash
nano prometheus.yml
```

**Zalijepite sljedeće:**

```yaml
global:
  scrape_interval: 30s
  evaluation_interval: 30s

scrape_configs:
  # Monitor Prometheus itself
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  # Monitor the monitoring server
  - job_name: 'monitoring-server'
    static_configs:
      - targets: ['node_exporter:9100']
```

**Spremite i izađite.**

### Korak 1.3: Stvaranje SNMP Exporter konfiguracije (zadana)

Preuzmite zadanu SNMP exporter konfiguraciju:

```bash
wget https://raw.githubusercontent.com/prometheus/snmp_exporter/main/snmp.yml
```

**Napomena:** Ova zadana konfiguracija uključuje MIB-ove za većinu uobičajenih mrežnih uređaja. Koristit ćemo je kakva jest za sada.

### Korak 1.4: Pokretanje stacka

```bash
# Preuzmite Docker slike
docker compose pull

# Pokrenite stack u pozadinskom načinu
docker compose up -d

# Provjerite da svi kontejneri rade
docker compose ps
```

**✅ Kontrolna točka:** Trebali biste vidjeti 3 kontejnera koji rade:
- `prometheus`
- `snmp_exporter`
- `node_exporter`

Svi bi trebali pokazivati status "Up".

### Korak 1.5: Provjera web pristupa

Iz web preglednika vašeg računala, idite na:

```
http://192.168.3.?:9090
```

Trebali biste vidjeti Prometheus web sučelje.

**✅ Kontrolna točka:**
1. Idite na **Status → Target health**
2. Trebali biste vidjeti 2 cilja:
   - `prometheus` (trebao bi biti UP)
   - `monitoring-server` (trebao bi biti UP)

---

## Misija 2: Otkrivanje mreže

**Cilj:** Pronaći IP adrese mrežnog hardwarea koristeći SNMP community string.

### Razumijevanje SNMP-a

Prije nego što počnemo skenirati, razumijte što tražite:
- **SNMP (Simple Network Management Protocol):** Protokol za prikupljanje informacija s mrežnih uređaja
- **Community String:** Kao lozinka za SNMP pristup (u našem slučaju: `Public_vtx`)
- **OID (Object Identifier):** Jedinstveni identifikator za specifičnu metriku (npr. `1.3.6.1.2.1.1.5.0` = hostname uređaja)

### Korak 2.1: Otkrijte uređaje s omogućenim SNMP-om

Umjesto skeniranja svih hostova, odmah pronađimo samo uređaje s otvorenim SNMP portom (UDP 161):

```bash
# Skenirajte za otvoreni SNMP port na cijelom lab rasponu
# -sU = UDP scan
# -p 161 = samo SNMP port
# --open = prikaži samo otvorene portove
sudo nmap -sU -p 161 --open 192.168.3.0/24
```

**Napomena:** UDP skeniranje može potrajati 1-2 minute. Budite strpljivi.

**📝 Dokumentirajte:** Zapišite sve IP adrese koje imaju otvoreni port 161/udp.

**Primjer izlaza:**
```
Nmap scan report for 192.168.3.1
PORT    STATE SERVICE
161/udp open  snmp

Nmap scan report for 192.168.3.2
PORT    STATE SERVICE
161/udp open  snmp
```

### Korak 2.2: Provjerite SNMP pristup s community stringom

Sada testirajte da li uređaji odgovaraju s pravim community stringom sa salvete (`Public_vtx`):

```bash
# Testirajte SNMP pristup za svaki pronađeni IP s portom 161
# Zamijenite <IP> sa svakom IP adresom koja ima port 161 otvoren
snmpwalk -v2c -c Public_vtx <IP> 1.3.6.1.2.1.1.5.0
```

**Primjer:**
```bash
snmpwalk -v2c -c Public_vtx 192.168.3.1 1.3.6.1.2.1.1.5.0
```

Ako je uspješno, vidjet ćete izlaz poput:
```
SNMPv2-MIB::sysName.0 = STRING: MikroTik-RB3011
```

**Ako dobijete "Timeout: No Response":**
- Uređaj možda koristi drugi community string
- SNMP možda nije potpuno konfiguriran
- Pokušajte sljedeći IP s liste

### Korak 2.3: Identificirajte vrste uređaja

Za svaki uređaj koji odgovara na SNMP, dobijte više informacija:

```bash
# Dobijte sistemski opis
snmpwalk -v2c -c Public_vtx <IP> 1.3.6.1.2.1.1.1.0

# Dobijte vrijeme rada sustava
snmpwalk -v2c -c Public_vtx <IP> 1.3.6.1.2.1.1.3.0
```

**✅ Kontrolna točka:** Trebali biste identificirati:
- MikroTik RB3011 (Router)
- MikroTik CSS326 (Switch)
- Cisco Catalyst 2950 (Switch)

**📝 Dokumentirajte:** Stvorite tablicu:

| Naziv uređaja | IP adresa | Vrsta | SNMP radi? |
|---------------|-----------|-------|------------|
| RB3011        |           | Router | Da/Ne     |
| CSS326        |           | Switch | Da/Ne     |
| Cisco 2950    |           | Switch | Da/Ne     |

---

## Misija 3: Konfiguracija SNMP nadzora

**Cilj:** Konfigurirati Prometheus da prikuplja metrike s vaših mrežnih uređaja putem SNMP Exportera.

### Razumijevanje arhitekture

Važan koncept: **Prometheus NE razgovara izravno s vašim switchevima.**

```
Prometheus → SNMP Exporter → Mrežni uređaj
```

- Prometheus prikuplja podatke od SNMP Exportera (HTTP na portu 9116)
- SNMP Exporter prevodi Prometheus zahtjeve u SNMP upite
- SNMP Exporter šalje SNMP upite uređaju
- Uređaj odgovara s SNMP podacima
- SNMP Exporter pretvara SNMP podatke u Prometheus metrike

### Korak 3.1: Ručno testirajte SNMP Exporter

Prije konfiguriranja Prometheusa, provjerite može li SNMP Exporter komunicirati s vašim uređajima:

```bash
# S vašeg VM-a, testirajte SNMP Exporter
# Zamijenite <DEVICE_IP> s jednim od vaših otkrivenih IP-a
curl 'http://localhost:9116/snmp?module=if_mib&target=<DEVICE_IP>'
```

**Primjer:**
```bash
curl 'http://localhost:9116/snmp?module=if_mib&target=192.168.3.1'
```

**✅ Kontrolna točka:** Trebali biste vidjeti hrpu metrika u Prometheus formatu:
```
# HELP ifHCInOctets The total number of octets received...
# TYPE ifHCInOctets counter
ifHCInOctets{ifDescr="ether1",ifIndex="1"} 1.234567e+09
...
```

Ako vidite ovo, vaš SNMP Exporter radi!

### Korak 3.2: Ažurirajte Prometheus konfiguraciju

Sada konfigurirajte Prometheus da prikuplja podatke s vaših uređaja. Uredite Prometheus konfiguraciju:

```bash
cd /home/lab/monitoring
nano prometheus.yml
```

**Dodajte sljedeće na dno datoteke (zadržite postojeće `scrape_configs`):**

```yaml
  # MikroTik RB3011 (Router)
  - job_name: 'mikrotik-rb3011'
    static_configs:
      - targets:
        - 192.168.3.X  # Zamijenite X stvarnom IP adresom koju ste otkrili
    metrics_path: /snmp
    params:
      module: [if_mib]
      auth: [public_v2]
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: snmp_exporter:9116

  # MikroTik CSS326 (Switch)
  - job_name: 'mikrotik-css326'
    static_configs:
      - targets:
        - 192.168.3.Y  # Zamijenite Y stvarnom IP adresom koju ste otkrili
    metrics_path: /snmp
    params:
      module: [if_mib]
      auth: [public_v2]
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: snmp_exporter:9116

  # Cisco Catalyst 2950 (Switch)
  - job_name: 'cisco-2950'
    static_configs:
      - targets:
        - 192.168.3.Z  # Zamijenite Z stvarnom IP adresom koju ste otkrili
    metrics_path: /snmp
    params:
      module: [if_mib]
      auth: [public_v2]
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: snmp_exporter:9116
```

**Važno:** Zamijenite IP adrese (`192.168.3.X`, `.Y`, `.Z`) stvarnim IP-ovima koje ste otkrili!

**Spremite i izađite.**

### Korak 3.3: Ažurirajte SNMP Exporter autentifikaciju

Zadana `snmp.yml` koristi community string `public`. Moramo je modificirati da koristi `Public_vtx`:

```bash
cd /home/lab/monitoring
nano snmp.yml
```

Pronađite sekciju koja izgleda ovako:
```yaml
  public_v2:
    community: public
    ...
```

Promijenite `public` u `Public_vtx`:
```yaml
  public_v2:
    community: Public_vtx
    ...
```

**Spremite i izađite.**

### Korak 3.4: Ponovno učitajte konfiguraciju

```bash
# Restartujte kontejnere da preuzmu nove konfiguracije
docker-compose restart
```

Pričekajte 30 sekundi, zatim provjerite:

```bash
docker-compose ps
```

Svi kontejneri bi i dalje trebali pokazivati "Up".

### Korak 3.5: Provjerite ciljeve

U vašem pregledniku, vratite se na:
```
http://192.168.3.10:9090/targets
```

**✅ Kontrolna točka:** Sada biste trebali vidjeti dodatne ciljeve:
- `mikrotik-rb3011`
- `mikrotik-css326`
- `cisco-2950`

Svi bi trebali pokazivati status "UP" (ovo može potrajati do 30 sekundi).

**Ako neki pokazuju "DOWN", idite na Vodič za rješavanje problema niže.**

---

## Misija 4: Nadzor vlastitog servera

**Cilj:** Osigurati da se server za nadzor sam nadzire.

Ovo bi već trebalo raditi iz naše početne konfiguracije, ali provjerimo.

### Korak 4.1: Provjerite Node Exporter

```bash
# Provjerite da Node Exporter izlaže metrike
curl http://localhost:9100/metrics | head -n 20
```

Trebali biste vidjeti metrike poput:
```
# HELP node_cpu_seconds_total Seconds the CPUs spent in each mode.
# TYPE node_cpu_seconds_total counter
node_cpu_seconds_total{cpu="0",mode="idle"} 12345.67
...
```

### Korak 4.2: Upitajte metrike u Prometheusu

U Prometheus web sučelju (`http://192.168.3.10:9090`):

1. Kliknite na **Query**
2. U polje za izraz, upišite: `node_cpu_seconds_total`
3. Kliknite **Execute**

**✅ Kontrolna točka:** Trebali biste vidjeti podatke metrika. Pokušajte ove upite:
- `node_memory_MemAvailable_bytes` - Dostupna memorija
- `node_filesystem_avail_bytes` - Dostupan prostor na disku
- `rate(node_cpu_seconds_total[5m])` - Stopa korištenja CPU-a

**📝 Dokumentirajte:** Pokrenite upit `up` u Prometheusu. Ovo pokazuje koji su target up (1) ili down (0). 

---

## Misija 5: Provjera i dokumentacija

**Cilj:** Dokazati da vaš sustav za nadzor radi i dokumentirati implementaciju.

### Korak 5.1: Završna provjera zdravlja

Pokrenite ove naredbe za provjeru:

```bash
# Provjerite da svi kontejneri rade
docker-compose ps

# Provjerite Prometheus logove (ne bi trebalo biti grešaka)
docker-compose logs prometheus | tail -n 50

# Provjerite SNMP Exporter logove
docker-compose logs snmp_exporter | tail -n 50
```

### Korak 5.2: Testirajte prikupljanje podataka

U Prometheus web sučelju, pokrenite ove upite za provjeru podataka sa svakog uređaja:

**Upit 1: Vrijeme rada uređaja**
```promql
sysUpTime / 100 / 60 / 60 / 24
```
Ovo pokazuje vrijeme rada u danima za sve SNMP uređaje.

**Upit 2: Status sučelja**
```promql
ifOperStatus
```
Ovo pokazuje operativni status svih sučelja (1=up, 2=down).

**Upit 3: Mrežni promet**
```promql
rate(ifHCInOctets[5m]) * 8
```
Ovo pokazuje dolazni promet u bitovima po sekundi.

**✅ Kontrolna točka:** Sva tri upita bi trebala vratiti podatke za vaše mrežne uređaje.

### Korak 5.3: Test trajnosti podataka

```bash
# Restartujte cijeli stack
docker-compose down
docker-compose up -d

# Pričekajte 30 sekundi
sleep 30

# Ponovno provjerite targete
curl http://localhost:9090/api/v1/targets | grep -c '"health":"up"'
```

**✅ Kontrolna točka:** Broj bi trebao odgovarati broju ciljeva koje ste konfigurirali (trebalo bi biti 5 ili više).

---

## Vodič za rješavanje problema

### Problem: SNMP Exporter pokazuje ciljeve kao DOWN

**Uzrok 1: Nepodudaranje community stringa**
```bash
# Provjerite radi li community string ručno
snmpwalk -v2c -c Public_vtx <DEVICE_IP> 1.3.6.1.2.1.1.5.0
```
Ako ovo ne uspije, SNMP community string uređaja možda nije `Public_vtx`.

**Uzrok 2: Firewall blokira SNMP (port 161)**
```bash
# Testirajte UDP port 161
nmap -sU -p 161 <DEVICE_IP>
```
Trebalo bi pokazati `161/udp open`.

**Uzrok 3: Kriva IP adresa u prometheus.yml**
```bash
# Provjerite je li IP dostupan
ping <DEVICE_IP>
```

**Uzrok 4: SNMP nije omogućen na uređaju**
- Na MikroTiku: Provjerite je li SNMP omogućen u RouterOS-u
- Na Ciscu: Provjerite `show snmp` izlaz

### Problem: Docker kontejneri se stalno restartiraju

```bash
# Provjerite logove za greške
docker-compose logs <container_name>

# Uobičajeni problemi:
# - Nevažeća YAML sintaksa u konfiguracijskim datotekama
# - Port se već koristi
# - Nedovoljan prostor na disku
```

### Problem: Prometheus web sučelje se ne učitava

```bash
# Provjerite sluša li Prometheus
netstat -tulpn | grep 9090

# Provjerite firewall
ufw status

# Ako firewall blokira, dopustite port
ufw allow 9090/tcp
```

### Problem: "Permission denied" greške s Dockerom

```bash
# Dodajte svog korisnika u docker grupu (ako ne koristite root)
usermod -aG docker $USER
# Odjavite se i ponovno prijavite da ovo stupi na snagu
```

### Problem: SNMP Exporter vraća prazne metrike

Provjerite parametar modula u `prometheus.yml`. Za većinu mrežnih uređaja, koristite `if_mib` (interface MIB).

Za MikroTik-specifične metrike, pokušajte `mikrotik` modul.
Za Cisco-specifične metrike, pokušajte `cisco_wlc` ili standardni `if_mib`.

---

## Uvjeti za predaju

### Potrebni sadržaji

Stvorite jedan PDF dokument koji sadrži:

#### 1. Naslovnu stranicu
- Vaše ime i broj indeksa
- Naziv vježbe: "1. tjedan - Operation Dark Network: Prikupljanje podataka"
- Datum predaje

#### 2. Dokumentaciju otkrivanja mreže
- Tablicu otkrivenih uređaja (IP, hostname, vrsta)
- Izlaze naredbi koji pokazuju uspješne SNMP upite

#### 3. Konfiguracijske datoteke
- Vaš cijeli `docker-compose.yml`
- Vaš cijeli `prometheus.yml`
- Dokaz modificiranog community stringa u `snmp.yml`

#### 4. Snimke zaslona
- Prometheus Targets stranicu koja pokazuje SVE ciljeve kao UP
- Prometheus Graf koji prikazuje barem jednu SNMP metriku
- Docker kontejnere koji rade (`docker-compose ps` izlaz)

#### 5. Upiti za provjeru
Za svaki upit niže, uključite snimku zaslona rezultata u Prometheusu:
- `up` - Pokazuje sve ciljeve
- `sysUpTime / 100 / 60 / 60 / 24` - Vrijeme rada uređaja u danima
- `rate(ifHCInOctets[5m]) * 8` - Mrežni promet

#### 6. Refleksija (maksimalno 500 riječi)
Odgovorite na ova pitanja:
- Koji je bio najizazovniji dio ove vježbe?
- Objasnite vlastitim riječima kako Prometheus prikuplja podatke s SNMP uređaja
- Što ste naučili o Dockeru i kontejnerizaciji?
- Kako se ovaj pristup nadzoru razlikuje od tradicionalnog nadzora temeljenog na syslogu?

#### 7. Dnevnik rješavanja problema
Dokumentirajte barem 2 problema s kojima ste se susreli i kako ste ih riješili.

### Rok za predaju

**Rok:** Kraj laboratorijske sesije 1. tjedna (petak, 23:59)

**Predati na:** [Vaš sustav za upravljanje kolegijima / email]

---

## Dodatni resursi

### Osnove SNMP-a
- [Understanding SNMP](https://www.paessler.com/it-explained/snmp)
- [Common SNMP OIDs](https://www.alvestrand.no/objectid/1.3.6.1.2.1.html)

### Prometheus dokumentacija
- [Službena Prometheus dokumentacija](https://prometheus.io/docs/introduction/overview/)
- [Osnove PromQL-a](https://prometheus.io/docs/prometheus/latest/querying/basics/)

### SNMP Exporter
- [SNMP Exporter GitHub](https://github.com/prometheus/snmp_exporter)
- [Generator dokumentacija](https://github.com/prometheus/snmp_exporter/tree/main/generator)

### Docker Compose
- [Compose file referenca](https://docs.docker.com/compose/compose-file/)

---

## Što je sljedeće?

**Najava 2. tjedna:** Sada kada imate prikupljanje podataka koje radi, sljedeći tjedan ćete postaviti **Grafanu** i izgraditi vizualne dashboardove kako biste ove podatke učinili razumljivima za netehničke dionike.

Transformirat ćete sirove SNMP metrike u vizualizacije prilagođene rukovodstvu koje odgovaraju na pitanja poput:
- "Kojem uređaju će uskoro ponestati propusnosti?"
- "Koliki je postotak dostupnosti naše mreže?"
- "Je li skladišni switch zdrav?"

Održavajte svoj VM pokrenutim i neka prikuplja podatke. Što više povijesti imate, to će vaši grafovi izgledati bolje!

---

**Sretno, mrežni administratore. Tvrtka računa na vas.**
