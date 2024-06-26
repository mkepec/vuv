# Instalacija Grafana Servera na Ubuntu

## Instalacija Grafana servera
Pratite korake za instalaciju Grafana servera na Ubuntu virtualni server


1. Install the prerequisite packages:
```
sudo apt-get install -y apt-transport-https software-properties-common wget
```

2. Import the GPG key:
```
sudo mkdir -p /etc/apt/keyrings/
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null
```

3. To add a repository for stable releases, run the following command:
```
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
```

4. Run the following command to update the list of available packages:
```
sudo apt-get update
```

5. To install Grafana OSS, run the following command:
```
sudo apt-get install grafana
```

6. Pokrenite Grafana servis:
```
sudo systemctl start grafana-server
```

7. Provjerite Grafana servis
```
sudo systemctl status grafana-server
```

8. Omogućite automatsko pokretanje Grafana servera pri pokretanju sistema:
```
sudo systemctl enable grafana-server
```


## Povezivanje na Grafana Server
Nakon instalacija Grafana servera i provjere kako je servis ispravno pokrenut, možete se povezati na Grafanu putem ovog URLa:
http://localhost:3000

Koristite ove podatke za prijavu:
username: `admin`
password: `admin`


## Dodavanje Prometheusa kao izvora podataka u Grafani
1. Prijavite se u Grafana web sučelje:
- Otvorite preglednik i idite na http://localhost:3000.
- Prijavite se koristeći zadane vjerodajnice (korisničko ime: `admin`, lozinka: `admin`).
- Promijenite lozinku prilikom prvog prijavljivanja. (nije obavezno)

2. Dodajte Prometheus kao data source:
- Idite na Configuration (ikona zupčanika) > Data Sources.
- Kliknite Add data source.
- Odaberite Prometheus sa liste.
- Unesite URL vašeg Prometheusa, npr. http://localhost:9090.
- Kliknite Save & Test kako biste provjerili konekciju.


## Dodavanje Grafana Dashboarda
Nakon dodavanja Prometheus servera kao izvora podataka (Data Source) za vaš Grafana server, možete dodati neki od već kreiranih Dashboarda koje možete preuzeti s ove stranice https://grafana.com/grafana/dashboards/.
- Pronađite primjere Dashboarda kojima je izvor podataka Prometheus, a pronađite Dashboarde za Node Exporter koji će vizualizirate metrike vašeg Ubuntu virtualnog servera, kao i Windows Exporter koji će vizulizirati metrike vašeg Windows računala u laboratoriju. 
- Za navedene Dashboarde kopirajte URL ili samo zabilježite ID Dashoarda
Preduvijeti za vizualizaciju metrika tim Grafana Dashoboardima je instaliran Node Exporter na Ubuntu VM, kao i instaliran [Windows Exporter](https://github.com/prometheus-community/windows_exporter/releases/tag/v0.25.1) (preuzmite i instalirajte `.msi`)

Dodavanje Dashboarda u Grafanu napravite pomoću ovih koraka:
1. Idite na + (Add) > `Import`.
2. U polje Import via grafana.com unesite ID dashboarda.
3. Kliknite `Load`.
4. Odaberite Prometheus data source koji ste ranije dodali.
5. Kliknite `Import`.


Nakon ovih koraka, trebali biste imati instaliranu i konfiguriranu Grafanu, s dodanim Prometheus data sourceom i primjer dashboardovima za Node Exporter i Windows Exporter. Sada možete početi nadzirati svoje sustave koristeći Grafana dashboarde.






