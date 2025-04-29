📄 Osvrt na prvotno priloženi prijedlog predmeta („Upravljanje kibernetičkom sigurnošću“)
Prvotni dokument (PDF) sadrži dobro strukturiran plan predavanja i vježbi raspoređenih kroz 15 tjedana s 2+2 sata tjedno. Nudi široki spektar tema koje obuhvaćaju sve bitne dijelove kibernetičke sigurnosti: standarde, mrežnu i aplikacijsku sigurnost, IAM, incident response, cloud i OT sigurnost.

✅ Prednosti:
Obuhvaća ključne teme kibernetičke sigurnosti.

Kombinira teoriju i praksu kroz laboratorijske vježbe.

Dotiče se aktualnih i modernih aspekata sigurnosti (npr. DevSecOps, cloud, OT).

⚠️ Ograničenja:
Teme su ponekad opisane vrlo kratko bez jasnog cilja predavanja/vježbe.

Nema eksplicitnih ishoda učenja po tjednu.

Ne postoji jasan prijedlog za završni projekt.

Labovi su navedeni, ali nisu dosljedno povezani s teorijom i ne uključuju dodatne izazove ili mogućnosti.

🆕 Poboljšanja u novoj verziji
Nova verzija silabusa značajno proširuje i unaprjeđuje izvorni prijedlog na sljedeće načine:

🔹 Strukturiranije:
Jasno raspoređeni tjedni moduli s opisima predavanja i laboratorijskih vježbi.

Svaki tjedan povezan s jednim ili više općih ishoda učenja (njih 8), što olakšava vrednovanje i usklađivanje sa standardima visokog obrazovanja.

🔹 Opsežnija i realističnija laboratorijska komponenta:
Uvedeni su konkretni alati (pfSense, Burp Suite, OpenSSL, ELK stack, Metasploit itd.).

Predložena je integracija AWS cloud resursa za izvođenje određenih vježbi (npr. IAM, CloudTrail, VPC).

Uvedene su mogućnosti za dodatne bodove i samostalni mini-projekt u završnim tjednima.

🔹 Tematski fokusiranije:
Teme su pažljivije raspoređene: uvod, standardi, operativna sigurnost, razvoj, specifična okruženja (cloud, OT, IoT), etika, projekt.

Obuhvaćene su ključne teme koje su nedostajale ili bile površno obrađene u originalu, poput:

Threat modeling

DevSecOps i SAST/DAST

SOC/SIEM pristup kroz ELK

Penetration testing faze i izvještavanje

❌ Izbačeno ili smanjeno u fokusu:
Niti jedna ključna tema nije potpuno izbačena, ali su neki sadržaji reorganizirani:

Teme poput sigurnosnih okvira i politika sada su koncentrirane u prva tri tjedna.

Sigurnost baza podataka nije posebno naglašena — može se uključiti kao dodatni zadatak ako bude interesa.

✅ Zaključak
Nova verzija je:

didaktički kvalitetnija

praktično orijentiranija

usklađena s trendovima i tržišnim potrebama

otvorena za proširenja (AWS, dodatne vježbe, projekti)

Ako želiš, mogu sada pripremiti i sve kao formalni predložak za ISVU/ECTS dokumentaciju (s opisom cilja predmeta, sadržajem po tjednima, ishodima i vrstama provjere znanja). Želiš li to?



# Predmet: Upravljanje kibernetičkom sigurnošću

**Razina:** Diplomski studij, smjer programsko inženjerstvo  
**Tjedna satnica:** 2 sata predavanja + 2 sata laboratorijskih vježbi  
**Trajanje:** 15 tjedana

---

## Opći ishodi učenja predmeta

1. Prepoznati prijetnje, ranjivosti i rizike u informacijskim sustavima.
2. Primijeniti sigurnosne standarde i politike u praksi.
3. Analizirati mrežne i sustavske zapise radi otkrivanja sigurnosnih incidenata.
4. Konfigurirati osnovne sigurnosne alate i sustave.
5. Primijeniti sigurne prakse u razvoju softvera.
6. Procijeniti sigurnost različitih IT okruženja (cloud, mobilno, IoT, OT).
7. Izraditi plan odgovora na sigurnosne incidente.
8. Primijeniti etiku i zakonodavni okvir u domeni kibernetičke sigurnosti.

---

## Tjedni raspored tema i vježbi

### Tjedan 1: Uvod u kibernetičku sigurnost
- **Predavanje:** Osnovni pojmovi, CIA trijada, aktualni napadi, uvod u sigurnosne pristupe.
- **Vježbe:** Postavljanje sigurnosnog lab okruženja (VM, mreža).
- **Ishodi učenja:** 1, 4

### Tjedan 2: Upravljanje rizicima i standardi
- **Predavanje:** ISO 27001/NIST CSF, matrica rizika.
- **Vježbe:** Modeliranje rizika, analiza slučaja.
- **Ishodi učenja:** 1, 2

### Tjedan 3: Sigurnosne politike i IAM
- **Predavanje:** Politike, RBAC, MFA, GPO, IAM koncepti.
- **Vježbe:** Upravljanje korisnicima i grupama (Linux/Windows), primjena pravila.
- **Ishodi učenja:** 2, 4

### Tjedan 4: Kriptografija u praksi
- **Predavanje:** Kriptografski algoritmi, PKI, potpisi, CA.
- **Vježbe:** OpenSSL, izrada CA, šifriranje i potpisivanje.
- **Ishodi učenja:** 2, 4

### Tjedan 5: Mrežna sigurnost I
- **Predavanje:** TCP/IP, vatrozidi, IDS/IPS, OSI.
- **Vježbe:** pfSense, Snort pravila.
- **Ishodi učenja:** 1, 4

### Tjedan 6: Mrežna sigurnost II
- **Predavanje:** VPN, segmentacija, NAC.
- **Vježbe:** Postavljanje VPN tunela, test pristupa.
- **Ishodi učenja:** 4

### Tjedan 7: Sigurnost aplikacija
- **Predavanje:** OWASP Top 10, sigurno kodiranje.
- **Vježbe:** SQLi, XSS, Burp Suite.
- **Ishodi učenja:** 5

### Tjedan 8: DevSecOps i sigurnost u razvoju
- **Predavanje:** SDLC, CI/CD sigurnost, SAST/DAST alati.
- **Vježbe:** SAST u CI (npr. SonarQube), analiza ranjivosti.
- **Ishodi učenja:** 5, 2

### Tjedan 9: Upravljanje incidentima
- **Predavanje:** Incident lifecycle, forenzika, BCP/DRP.
- **Vježbe:** Analiza logova, ELK stack, IOC prepoznavanje.
- **Ishodi učenja:** 3, 7

### Tjedan 10: Cloud sigurnost
- **Predavanje:** AWS/Azure sigurnosni alati, IAM, alerting.
- **Vježbe:** Cloud sandbox konfiguracija i analiza logova.
- **Ishodi učenja:** 6, 4

### Tjedan 11: OT i ICS sigurnost
- **Predavanje:** SCADA, Modbus, razlike IT/OT.
- **Vježbe:** Emulacija SCADA, firmware analiza.
- **Ishodi učenja:** 1, 6

### Tjedan 12: Sigurnost mobilnih i IoT sustava
- **Predavanje:** MDM, OTA napadi, endpoint zaštita.
- **Vježbe:** Analiza prometa IoT uređaja, šifriranje, ranjivosti.
- **Ishodi učenja:** 6

### Tjedan 13: Penetration testing i red teaming
- **Predavanje:** OSINT, recon, exploitation, izvještavanje.
- **Vježbe:** Metasploit lab, kratki pentest izvještaj.
- **Ishodi učenja:** 1, 4

### Tjedan 14: Etika i zakonodavni okvir
- **Predavanje:** GDPR, etika, zakonodavni aspekti hakiranja.
- **Vježbe:** Rasprava, studije slučaja.
- **Ishodi učenja:** 8

### Tjedan 15: Završna vježba ili projekt
- **Predavanje:** Refleksija, prezentacije, retrospektiva predmeta.
- **Vježbe:** Prezentacija mini projekta (obrana sustava, analiza incidenta, hardening).
- **Ishodi učenja:** Svi

---

## Dodatne aktivnosti

- **Dodatni bodovi:** Prezentacija dodatne teme (sigurnost kontejnera, analiza malvera).
- **Mini projekt:** Samostalno ili u paru – razvoj sigurnosnog rješenja, skripte ili dokumentacije (npr. hardening vodič za sustav).
