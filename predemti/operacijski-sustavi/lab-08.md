# 💻 Laboratorijska vježba: Rad s EC2 instancom i administracija Amazon Linux 2

Ova laboratorijska vježba vodi studente kroz rad s EC2 instancom u oblaku koristeći Amazon Linux 2. Kroz niz edukativnih poglavlja i praktičnih koraka, studenti će naučiti kako zaštititi instancu, proširiti disk, koristiti osnovne Linux naredbe, upravljati softverskim paketima, arhivirati datoteke, uređivati sistemske datoteke i pisati jednostavne bash skripte.

Vježba se izvodi na prethodno pripremljenoj EC2 instanci s Amazon Linux 2 AMI-em.  
Preduvjeti:
- ✅ [Pokretanje EC2 instance](#) *(poveznica na odvojeni vodič)*
- ✅ [Spajanje putem SSH-a](#) *(poveznica na odvojeni vodič)*

---

## 📚 Sadržaj

- [1. Zaštita EC2 instance](#-zaštita-ec2-instance-od-slučajnog-gašenja-i-brisanja)
- [2. Proširenje diska instance](#-proširenje-diska-ec2-instance-ebs-volume)
- [3. Osnovne naredbe u Linux CLI-ju](#-osnovne-naredbe-u-linux-komandnoj-liniji-cli)
- [4. Upravljanje paketima i repozitorijima](#-upravljanje-repozitorijima-i-paketima-na-amazon-linux-2)
- [5. Arhiviranje i sažimanje datoteka](#-arhiviranje-i-sažimanje-datoteka-i-direktorija)
- [6. Rad s vim editorom](#-rad-s-vim-editorom-i-uređivanje-konfiguracijskih-datoteka)
- [7. Bonus zadatak – Bash skripting](#-bonus-zadatak--uvod-u-bash-skripting-i-sistemski-nadzor)
- [📌 Završni popis zadataka](#-završni-popis-zadataka-za-samostalni-rad)


# 🛡️ Zaštita EC2 instance od slučajnog gašenja i brisanja

## 🎓 Uvod

U radu s virtualnim strojevima u oblaku, poput EC2 instanci u AWS-u, vrlo je važno osigurati njihovu dostupnost i stabilnost, osobito kada se koriste za produkcijske aplikacije, baze podataka ili sustave koji sadrže važne korisničke podatke. U takvim scenarijima, slučajno gašenje ili brisanje instance može imati ozbiljne posljedice – gubitak podataka, prekid rada servisa ili sigurnosne propuste.

AWS omogućuje dodatne sigurnosne mehanizme za sprječavanje slučajnog **gašenja (stop)** ili **brisanja (terminate)** instanci putem atributa koji se zovu **Instance Stop Protection** i **Termination Protection**. Kada su ti atributi omogućeni, korisnik mora eksplicitno isključiti zaštitu prije nego što može zaustaviti ili obrisati instancu – što stvara dodatni sloj sigurnosti.

Ova opcija se najčešće koristi za:
- **produkcijske servere** koji moraju biti neprekidno dostupni,
- **sustave s kritičnim podacima** koji nisu automatski sigurnosno kopirani,
- **učenike i početnike**, kako bi se spriječile greške tijekom laboratorijskih vježbi.

> **Napomena:** Ove opcije **ne štite od rušenja instance** zbog vanjskih razloga (poput problema u zoni dostupnosti), već isključivo od **neopreznog korisničkog ponašanja** u konzoli ili putem CLI-ja.

## 🎯 Cilj

Omogućiti zaštitu instance od slučajnog gašenja (Stop) i brisanja (Terminate) putem AWS konzole.

## 🧪 Upute

### 🔹 Koraci za uključivanje zaštite instance:

1. U AWS konzoli otvori **EC2 Dashboard**.
2. U lijevom izborniku klikni na **Instances**, pa odaberi svoju instancu.
3. U gornjem izborniku klikni na **Actions > Instance settings > Change termination protection**.
4. Označi **Enable** i klikni **Save**.

Isti postupak ponovi za **Change stop protection** kako bi se spriječilo zaustavljanje instance:

5. Klikni na **Actions > Instance settings > Change stop protection**.
6. Označi **Enable** i spremi promjene.

## ✅ Provjera

Pokušaj ugasiti instancu:
1. Odaberi instancu.
2. Klikni na **Instance state > Stop instance**.
3. Trebao bi se prikazati tekstualni odgovor poput:

```
Failed to stop the instance i-xxxxxxxxxxx. The instance 'i-xxxxxxxxxxx' may not be stopped. Modify its 'disableApiStop' instance attribute and try again.
```

Ova poruka potvrđuje da je zaštita od gašenja aktivna.

## 📌 Onemogućavanje zaštite

Za potrebe testiranja ili uklanjanja instance, možeš onemogućiti zaštitu:

1. **Actions > Instance settings > Change stop/termination protection**
2. **Ukloni kvačicu** s Enable
3. Klikni **Save**

## 🧠 Savjeti

> **Tip:** Uključivanje zaštite ne zahtijeva ponovno pokretanje instance i može se uključiti ili isključiti bilo kada.

> **Napomena:** Ova zaštita **ne sprječava** gašenje instance ako dođe do prekida usluge u AWS-u, promjene stanja zbog skaliranja ili planiranog održavanja.



# 📦 Proširenje diska EC2 instance (EBS volume)

## 🎓 Uvod

Tijekom životnog ciklusa jedne EC2 instance može se dogoditi da količina slobodnog prostora na disku više nije dovoljna. To se može dogoditi zbog:
- **rastučeg broja logova ili podataka** koje aplikacija pohranjuje na lokalni disk,
- **instalacije novih paketa i ovisnosti** koji zauzimaju dodatni prostor,
- **povećanja baze podataka ili temp fajlova**,
- **promjene u poslovnim zahtjevima** koji traže više prostora bez zamjene instance.

U AWS-u, EC2 instance koriste **Elastic Block Store (EBS)** – mrežno povezane virtualne diskove koji se mogu dinamički proširivati. To omogućuje jednostavno povećanje diska bez potrebe za migracijom instance ili podataka.

> **Napomena:** Proširenje volumena ne smije se zamijeniti s dodavanjem novog EBS volumena – ovdje govorimo o povećanju veličine **postojećeg** korijenskog (root) volumena instance.

## 🎯 Cilj

Povećati veličinu root diska instance (EBS volumena) i proširiti datotečni sustav unutar instance kako bi se novi prostor mogao koristiti.

## 📚 Važne napomene prije početka

- Instancu je potrebno **zaustaviti (Stop)** prije proširenja instance tipa (npr. s `t2.micro` na `t2.small`), ali **proširenje EBS volumena može se napraviti bez gašenja**.
- Nakon promjene veličine volumena putem konzole, dodatno je potrebno unutar instance **proširiti particiju** i **datotečni sustav** da bi se prošireni prostor koristio.

## 🧪 Upute

### 1. Provjeri trenutno stanje diska i korištenje

Spoji se na instancu putem SSH i pokreni:
```bash
lsblk
df -h
```

> `lsblk` prikazuje fizičke uređaje i particije  
> `df -h` prikazuje korištenje prostora po montiranim datotečnim sustavima

Zapamti naziv diska, najčešće `/dev/xvda` i particije `/dev/xvda1`.

### 2. Proširi volumen u AWS konzoli

1. U AWS konzoli idi na **EC2 > Instances > Storage > Volume ID**.
2. Klikni na **Volume ID** da otvoriš stranicu volumena.
3. Klikni na **Actions > Modify volume**.
4. U polje **Size (GiB)** unesi novu vrijednost, npr. `10`.
5. Klikni **Modify > Modify** da potvrdiš promjenu.

> **Tip:** AWS automatski povećava volumen u pozadini bez prekida rada instance.

### 3. Proširi particiju i datotečni sustav

Povratak u SSH sesiju:

#### Proširi particiju pomoću `growpart`:
```bash
sudo growpart /dev/xvda 1
```

#### Proširi datotečni sustav (XFS):
```bash
sudo xfs_growfs -d /
```

> **Napomena:** Amazon Linux 2 koristi XFS kao zadani datotečni sustav. Ako je u pitanju `ext4`, naredba bi bila `resize2fs`.

## ✅ Provjera

Pokreni ponovno:
```bash
df -h
```

Provjeri je li novi kapacitet vidljiv i dostupan na `/`.

## 🧠 Savjeti

> **Tip:** Uvijek napravi backup važnih podataka prije promjena na disku.

> **Napomena:** Ako je volumen proširen, ali particija ili datotečni sustav nisu, dodatni prostor neće biti dostupan dok se ručno ne proširi.

> **Instruktorski savjet:** Studenti mogu simulirati pun disk kreiranjem velikih datoteka:
```bash
fallocate -l 1G test1.img
```



# 💻 Osnovne naredbe u Linux komandnoj liniji (CLI)

## 🎓 Uvod

Linux komandna linija (CLI – *Command Line Interface*) jedan je od najmoćnijih alata koje sistemski administratori, DevOps inženjeri i programeri koriste za upravljanje sustavima. Za razliku od grafičkog sučelja (GUI), rad u CLI-ju omogućuje veću fleksibilnost, automatizaciju i pristup funkcionalnostima koje su često nedostupne u grafičkom okruženju.

Rad u komandnoj liniji zahtijeva određeno učenje sintakse, ali jednom kada savladate osnovne naredbe, otvaraju se mnoge mogućnosti za brže, efikasnije i preciznije upravljanje serverima i aplikacijama.

> **Napomena:** U svijetu oblaka, većina poslužitelja (uključujući EC2 instance) dolazi bez grafičkog sučelja, stoga je znanje rada u terminalu nužno za svakog cloud korisnika.

## 🎯 Cilj

Naučiti osnovne naredbe za navigaciju, upravljanje datotekama, pregled sadržaja, pretragu i traženje pomoći u Linuxu.

---

## 📂 Navigacija po datotečnom sustavu

| Naredba           | Opis                                      |
|------------------|-------------------------------------------|
| `pwd`            | Prikazuje trenutnu lokaciju (putanju)     |
| `cd <mapa>`      | Ulazak u mapu                             |
| `cd ..`          | Povratak na mapu iznad                    |
| `ls`             | Ispis sadržaja mape                       |
| `ls -l`          | Ispis u obliku liste s detaljima          |

### ✅ Vježba:
```bash
pwd
cd /tmp
ls -l
cd ~
```

---

## 📁 Upravljanje datotekama i mapama

| Naredba                        | Opis                                     |
|-------------------------------|------------------------------------------|
| `touch naziv.txt`             | Kreira praznu datoteku                   |
| `mkdir nova_mapa`             | Kreira novi direktorij                   |
| `cp izvor.txt kopija.txt`     | Kopira datoteku                          |
| `mv ime.txt nova_mapa/`       | Premješta datoteku                       |
| `rm datoteka.txt`             | Briše datoteku                           |
| `rmdir prazna_mapa`           | Briše prazan direktorij                  |
| `file naziv.txt`              | Prikazuje tip datoteke                   |

### ✅ Vježba:
```bash
mkdir test
cd test
touch primjer.txt
cp primjer.txt kopija.txt
mv kopija.txt arhiva.txt
rm arhiva.txt
cd ..
rmdir test
```

---

## 📑 Pregled sadržaja datoteka

| Naredba               | Opis                                 |
|-----------------------|--------------------------------------|
| `cat ime.txt`         | Ispisuje cijeli sadržaj              |
| `less ime.txt`        | Pregled u "scrollable" načinu       |
| `head -n 10 ime.txt`  | Prvih 10 redaka                      |
| `tail -n 10 ime.txt`  | Zadnjih 10 redaka                   |

---

## 🔍 Pretraga i pronalaženje

| Naredba                             | Opis                                           |
|-------------------------------------|------------------------------------------------|
| `find / -name "ime.txt"`            | Traži datoteku prema imenu                    |
| `locate ime.txt`                    | Brza pretraga datoteka po indeksu (ako postoji) |
| `grep "tekst" ime.txt`              | Traži string unutar datoteke                  |

---

## 📘 Traženje pomoći

| Naredba               | Opis                              |
|-----------------------|-----------------------------------|
| `man <naredba>`       | Otvara priručnik za naredbu       |
| `naredba --help`      | Brzi pregled opcija naredbe       |

### ✅ Vježba:
```bash
man ls
ls --help
```

> **Tip:** U `man` sučelju, koristi tipke `G` (kraj), `q` (izlaz), `/` (pretraga).

---

## 🧠 Savjeti

> **Tip:** Svaka naredba u Linuxu može imati desetke opcija – koristite `man` redovito da istražite dodatne mogućnosti.

> **Instruktorski savjet:** Potaknite studente da rješavaju zadatke koristeći samo `man` kako bi razvili naviku samostalnog istraživanja naredbi.



# 📦 Upravljanje repozitorijima i paketima na Amazon Linux 2

## 🎓 Uvod

Jedna od temeljnih značajki Linux sustava je fleksibilan sustav za upravljanje softverom pomoću **paketnih menadžera**. Umjesto da ručno preuzimamo i instaliramo programe, Linux distribucije koriste **repozitorije** – službene ili alternativne izvore softverskih paketa koji se mogu instalirati, nadograditi ili ukloniti pomoću jedne naredbe.

Amazon Linux 2 koristi **`dnf`** (nasljednik `yum`) kao glavni alat za upravljanje paketima. `dnf` omogućuje:
- instalaciju paketa iz službenih repozitorija,
- uklanjanje i ažuriranje paketa,
- pretraživanje paketa prema imenu ili opisu,
- upravljanje konfiguracijom repozitorija.

## 📚 Što su repozitoriji?

Repozitorij je kolekcija paketa (softverskih arhiva) dostupnih putem interneta. Svaki repozitorij ima vlastitu konfiguraciju spremljenu u direktoriju:

```bash
/etc/yum.repos.d/
```

U tim `.repo` datotekama definiraju se URL-ovi izvora, naziv repozitorija, prioriteti, GPG ključevi i druga pravila. Na Amazon Linux 2 sustavima su unaprijed definirani službeni repozitoriji kao što su:
- **amzn2-core** – glavni repozitorij sa stabilnim paketima,
- **amzn2extra** – dodatni repozitoriji s novijim verzijama softvera (mora se ručno uključiti).

## 🎯 Cilj

Naučiti upravljati softverskim paketima pomoću `dnf`, istražiti repozitorije i instalirati korisne alate.

## 🧪 Upute

### 1. Pregled repozitorija

```bash
dnf repolist
```

Prikazuje aktivne repozitorije s brojem dostupnih paketa.

> **Napomena:** Ako želiš dodatne pakete (npr. `nginx`, `php`), možda ćeš trebati omogućiti `amazon-linux-extras`.

```bash
amazon-linux-extras list
amazon-linux-extras enable nginx1
dnf install -y nginx
```

### 2. Instalacija paketa

```bash
sudo dnf install -y tree curl jq
```

| Paket  | Opis                                   |
|--------|----------------------------------------|
| `tree` | Hijerarhijski prikaz mapa i datoteka   |
| `curl` | Alat za HTTP zahtjeve iz terminala     |
| `jq`   | Alat za obradu i filtriranje JSON-a    |

#### ✅ Vježba:
```bash
tree ~
curl https://api.github.com
```

### 3. Pretraživanje i informacije

```bash
dnf search <pojam>
dnf info <paket>
```

Primjer:
```bash
dnf search zip
dnf info unzip
```

### 4. Uklanjanje paketa

```bash
sudo dnf remove tree
```

### 5. Ažuriranje sustava

```bash
sudo dnf update
```

> **Tip:** Redovito ažuriranje osigurava sigurnost i stabilnost sustava.

## 🧠 Savjeti

> **Napomena:** Ako želiš dodati vlastiti repozitorij (npr. interni ili treći izvor), možeš kreirati `.repo` datoteku u `/etc/yum.repos.d/` s konfiguracijom.

> **Instruktorski savjet:** Potakni studente da istraže kako se omogućava repozitorij `amzn2extra`, pronađu paket `git` i instaliraju ga.



# 🗜️ Arhiviranje i sažimanje datoteka i direktorija

## 🎓 Uvod

Arhiviranje i kompresija (sažimanje) koriste se svakodnevno u administraciji sustava, razmjeni podataka i izradi sigurnosnih kopija. U Linuxu, alatima iz komandne linije možemo:
- **arhivirati više datoteka u jednu** (`tar`),
- **komprimirati datoteke** radi uštede prostora (`gzip`, `bzip2`, `xz`),
- **raspakirati** ili pregledati sadržaj arhive bez vađenja.

Često se koristi kombinacija: prvo se više datoteka spoji u arhivu pomoću `tar`, zatim se ta arhiva sažima u `.gz`, `.bz2` ili `.xz` datoteku.

> **Napomena:** `.zip` format je popularan u Windows okruženjima, no i dalje podržan i u Linuxu putem alata `zip` i `unzip`.

## 🎯 Cilj

Naučiti arhivirati i komprimirati direktorije i datoteke, raspakirati ih i razumjeti razlike između dostupnih opcija.

## 🧪 Upute

### 1. Arhiviranje pomoću `tar`

#### Kreiranje `.tar` arhive (bez kompresije):
```bash
tar -cvf arhiva.tar direktorij/
```

| Opcija | Opis                       |
|--------|----------------------------|
| `-c`   | Create – kreiraj arhivu    |
| `-v`   | Verbose – ispiši sadržaj   |
| `-f`   | File – ime izlazne datoteke |

#### Pregled sadržaja `.tar` datoteke:
```bash
tar -tvf arhiva.tar
```

#### Ekstrakcija `.tar` arhive:
```bash
tar -xvf arhiva.tar
```

> **Tip:** Dodaj `-C mapa/` da raspakiraš sadržaj u određenu mapu.

---

### 2. Arhiviranje s kompresijom

#### `.tar.gz` ili `.tgz` (gzip):
```bash
tar -czvf arhiva.tar.gz direktorij/
tar -xzvf arhiva.tar.gz
```

#### `.tar.bz2` (bzip2):
```bash
tar -cjvf arhiva.tar.bz2 direktorij/
tar -xjvf arhiva.tar.bz2
```

#### `.tar.xz` (xz):
```bash
tar -cJvf arhiva.tar.xz direktorij/
tar -xJvf arhiva.tar.xz
```

| Kompresija | Alat     | Prednosti                  |
|------------|----------|----------------------------|
| gzip       | `gzip`   | Brza, dobra za logove      |
| bzip2      | `bzip2`  | Bolja kompresija, sporija  |
| xz         | `xz`     | Najveća kompresija, spora  |

---

### 3. ZIP i UNZIP

#### Komprimiranje:
```bash
zip -r arhiva.zip direktorij/
```

| Opcija | Opis                       |
|--------|----------------------------|
| `-r`   | Recursive – uključuje sve poddirektorije |

#### Dekompresija:
```bash
unzip arhiva.zip
```

---

### 4. Brza kompresija jedne datoteke

```bash
gzip ime.txt        # komprimira -> ime.txt.gz
gunzip ime.txt.gz   # dekompresira
```

> **Tip:** `gzip` briše izvornu datoteku nakon kompresije. Koristi `-k` za zadržavanje:
```bash
gzip -k ime.txt
```

---

## ✅ Vježba

1. Kreiraj direktorij i nekoliko datoteka:
```bash
mkdir primjer
touch primjer/{a.txt,b.txt,c.log}
```

2. Arhiviraj mapu:
```bash
tar -czvf primjer.tar.gz primjer/
```

3. Provjeri sadržaj:
```bash
tar -tzvf primjer.tar.gz
```

4. Raspakiraj u drugu mapu:
```bash
mkdir ekstrakt
tar -xzvf primjer.tar.gz -C ekstrakt/
```

---

## 🧠 Savjeti

> **Napomena:** Ako često arhiviraš logove, koristi `gzip` jer je brz i široko podržan.

> **Tip:** Dodaj u `.tar` i datum za automatsko imenovanje arhive:
```bash
tar -czvf backup_$(date +%F).tar.gz direktorij/
```



# 📝 Rad s `vim` editorom i uređivanje konfiguracijskih datoteka

## 🎓 Uvod

U Linux okruženju velik dio administracije uključuje **uređivanje konfiguracijskih datoteka** – od postavki servisa do skripti i korisničkih poruka. Iako postoji više tekstualnih editora, **`vim`** je jedan od najbržih, najmoćnijih i najraširenijih alata za rad u terminalu.

Naslijeđen iz starijeg `vi` editora, `vim` (Vi IMproved) donosi napredne mogućnosti poput višestrukih otvorenih datoteka, pretrage i zamjene, sintaksnog označavanja, rada u više prozora i automatskog uvučenja.

> **Napomena:** Iako `nano` editor može biti jednostavniji za početnike, `vim` je industrijski standard u mnogim profesionalnim okruženjima.

## 🎯 Cilj

Naučiti osnovne i napredne naredbe u `vim` editoru za učinkovito uređivanje datoteka na EC2 instanci s Amazon Linux 2.

---

## 📚 Instalacija i pokretanje

Amazon Linux 2 dolazi s `vi` editorom, no on je ograničen. Preporučuje se instalirati `vim`:

```bash
sudo dnf install -y vim
```

Pokretanje editora:
```bash
vim datoteka.txt
```

> **Tip:** Ako ti se otvori `vi`, ali želiš osigurati puni `vim`, koristi punu naredbu `vim`.

---

## 🧪 Osnove rada u `vim` editoru

### 🧭 Modovi u `vim`-u

`vim` koristi **tri glavna moda**:

| Mod         | Aktivacija      | Opis                                 |
|-------------|------------------|--------------------------------------|
| Normal      | automatski pri ulasku | Krećeš iz ovog moda – za kretanje i naredbe |
| Insert      | `i`, `a`, `o`    | Uređivanje teksta                    |
| Command     | `:`              | Spremanje, izlazak, traženje, zamjena |

> **Tip:** Za povratak iz bilo kojeg moda u Normal mod pritisni `Esc`.

---

### ✍️ Najvažnije naredbe

#### Navigacija:
- `h`, `j`, `k`, `l` – lijevo, dolje, gore, desno
- `w` – skok na sljedeću riječ
- `0` – početak reda
- `$` – kraj reda
- `gg` – početak datoteke
- `G` – kraj datoteke

#### Umetanje teksta:
- `i` – umetanje prije kursora
- `a` – umetanje nakon kursora
- `o` – novi red ispod
- `O` – novi red iznad

#### Brisanje:
- `x` – izbriši znak
- `dd` – izbriši red
- `dG` – izbriši do kraja datoteke

#### Kopiranje i lijepljenje:
- `yy` – kopiraj red
- `p` – zalijepi ispod
- `P` – zalijepi iznad

#### Spremanje i izlazak:
- `:w` – spremi
- `:q` – izađi
- `:wq` – spremi i izađi
- `:q!` – izađi bez spremanja

---

## 🧪 Vježba: Uređivanje `/etc/motd`

1. Pokreni uređivanje datoteke:
```bash
sudo vim /etc/motd
```

2. U Insert modu dodaj poruku dobrodošlice, npr.:
```
Dobrodošli na EC2 instancu za laboratorijsku vježbu!
```

3. Pritisni `Esc` za izlaz iz Insert moda.

4. Spremi i izađi:
```
:wq
```

5. Provjeri promjenu:
```bash
cat /etc/motd
```

---

## 🔍 Naprednije naredbe

| Naredba               | Opis                                     |
|-----------------------|------------------------------------------|
| `:set nu`             | Prikaz brojeva linija                    |
| `:syntax on`          | Uključi označavanje sintakse             |
| `:%s/staro/novo/g`    | Zamijeni sve pojave riječi               |
| `:split datoteka2`    | Otvori drugi fajl u horizontalnom splitu |
| `:e /putanja/do/fajla`| Otvori drugi fajl                        |
| `:w !sudo tee`        | Spremi kao root ako nisi pokrenuo s `sudo` |

> **Primjer:** Spremanje datoteke bez izlaska iz `vim`, iako nemaš root ovlasti:
```
:w !sudo tee %
```

---

## 🧠 Savjeti

> **Tip:** Koristi `vimtutor` (ako je instaliran) za interaktivno učenje `vim` naredbi.

> **Instruktorski savjet:** Iako `vim` može djelovati neintuitivno na početku, njegova snaga dolazi do izražaja s praksom. Potakni studente da uređuju više vrsta datoteka: skripte, konfiguracije, logove.



# 🎁 Bonus zadatak – Uvod u bash skripting i sistemski nadzor

## 🎓 Uvod u bash skripting

U Linux okruženju, **bash skripte** su izvršne tekstualne datoteke koje sadrže niz naredbi koje se izvršavaju jedna za drugom. Skripting omogućuje:
- **automatizaciju ponavljajućih zadataka** (npr. ažuriranje sustava, sigurnosne kopije),
- **kreiranje alata** specifičnih za vlastite potrebe,
- **poboljšanje produktivnosti** administracije sustava.

Bash skripte se izvršavaju pomoću **Bash ljuske** (engl. *Bourne Again Shell*), a gotovo svi Linux sustavi, uključujući Amazon Linux 2, koriste bash kao zadanu ljusku.

---

## 📚 Struktura bash skripte

```bash
#!/bin/bash

# Ovo je komentar
echo "Ova skripta prikazuje sistemske informacije."
uptime
df -h
free -m
```

- **Shebang (`#!/bin/bash`)** – prvi redak koji govori sustavu kojom ljuskom da izvrši skriptu.
- **Komentari (`#`)** – objašnjenja koja se ignoriraju prilikom izvršavanja.

---

## 🧪 Kreiranje i izvršavanje skripte

### 1. Kreiraj skriptu:
```bash
vim monitor.sh
```

Dodaj sljedeći sadržaj:
```bash
#!/bin/bash

echo "Datum i vrijeme:"
date

echo "Uptime:"
uptime

echo "Iskorištenost diska:"
df -h

echo "Stanje memorije:"
free -m
```

### 2. Spremi i zatvori skriptu (`:wq`)

### 3. Učini skriptu izvršnom:
```bash
chmod +x monitor.sh
```

### 4. Pokreni skriptu:
```bash
./monitor.sh
```

> **Napomena:** Ako je skripta spremljena na lokaciji koja nije u `$PATH`, mora se pozivati s `./`.

---

## 🧠 Dodatne napomene

- Skripte se mogu raspoređivati za automatsko pokretanje pomoću `cron`.
- Korisno je dodavati `set -e` ili `set -x` na početak skripte za otkrivanje grešaka i debuggiranje.
- Svaka skripta treba imati jasan cilj i dokumentaciju (komentare).

---

## 🎯 Bonus zadatak

### ✔️ Zadatak: Napravi bash skriptu za sistemski nadzor

**Naziv skripte:** `system-check.sh`

### 🔧 Skripta mora:

1. Prikazati:
   - trenutno vrijeme (`date`)
   - podatke o korištenju CPU-a i memorije (`top` ili `uptime`, `free`)
   - zauzeće diska (`df -h`)
   - informacije o korisnicima (`who`, `w`)
2. Spremiti izlaz svih naredbi u datoteku `system_report.txt` u korisničkom direktoriju.

### 📦 Dodatno (napredno):
- Dodaj funkciju za svaki segment izvještaja
- Dodaj parametar `--save` koji dodatno arhivira izvještaj (koristi `tar` ili `gzip`)

#### Primjer izvršavanja:
```bash
./system-check.sh --save
```

---

## ✅ Primjer rješenja (osnovni oblik)

```bash
#!/bin/bash

echo "Sistemski izvještaj: $(date)" > ~/system_report.txt
echo "" >> ~/system_report.txt

echo "Uptime:" >> ~/system_report.txt
uptime >> ~/system_report.txt
echo "" >> ~/system_report.txt

echo "Disk:" >> ~/system_report.txt
df -h >> ~/system_report.txt
echo "" >> ~/system_report.txt

echo "Memorija:" >> ~/system_report.txt
free -m >> ~/system_report.txt
echo "" >> ~/system_report.txt

echo "Korisnici:" >> ~/system_report.txt
who >> ~/system_report.txt

# Napredno: arhiviranje ako je --save argument
if [[ "$1" == "--save" ]]; then
    tar -czf ~/system_report_$(date +%F).tar.gz ~/system_report.txt
    echo "Izvještaj arhiviran."
fi
```

---

> Čestitke! Ovo poglavlje spaja sve što ste naučili do sada: rad s naredbama, uređivanje datoteka, upotrebu `vim` editora, upravljanje diskovima i osnovni scripting.



# 📌 Završni popis zadataka za samostalni rad

## 1. Zaštita EC2 instance
- Pokušaj ugasiti instancu s uključenom zaštitom i analiziraj poruku.
- Onemogući zaštitu, ponovi gašenje i usporedi razliku u ponašanju.

## 2. Proširenje diska
- Proširi root volumen instance na 10 GiB.
- Proširi particiju i datotečni sustav iznutra.
- Provjeri zauzeće diska prije i nakon proširenja.

## 3. Osnovne naredbe u Linux CLI-ju
- Kreiraj strukturu direktorija i nekoliko datoteka, premještaj ih i briši.
- Iskoristi `man` za istraživanje naredbi `rmdir`, `file`, `tail`.
- Koristi `grep` i `find` za pretragu sadržaja.

## 4. Upravljanje repozitorijima i paketima
- Instaliraj `jq`, `curl`, `htop`.
- Pronađi dodatni repozitorij putem `amazon-linux-extras` i instaliraj `nginx`.
- Pronađi i ukloni neki nepotreban paket.

## 5. Arhiviranje i sažimanje
- Arhiviraj direktorij s `.tar`, `.tar.gz`, `.zip`.
- Isprobaj kompresiju i dekompresiju s `gzip` i `unzip`.
- Raspakiraj arhivu u novu mapu.

## 6. Vim editor
- Uredi `/etc/motd` i dodaj vlastitu poruku.
- Koristi `:%s` za zamjenu teksta unutar datoteke.
- Isprobaj rad s više datoteka u split prozoru.
- Spremi izmjene koristeći `:w !sudo tee %` bez zatvaranja editora.

## 7. Bash skripting (bonus)
- Napiši skriptu `monitor.sh` koja prikazuje sistemske informacije.
- Dodaj argument `--save` koji arhivira izlaz u `.tar.gz`.
- Organiziraj kod u funkcije.
- Pokreni skriptu i provjeri generirani izvještaj i arhivu.
