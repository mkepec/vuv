# 📊 Tjedan 1: Procjena Trenutnog Stanja VelocityTech-a
## *"Dobrodošli u Stvarni Svijet - Dokumentiranje Kaosa"*

---

*← [Nazad na Uvodnu Scenu](../story/opening-scene-hr.md) | [Dalje: Tjedan 2 Kriza Version Control-a →](week02-version-control-crisis-hr.md)*

---

> *"Ne možete poboljšati ono što ne mjerite. A ne možete mjeriti ono što ne vidite. Ovaj tjedan, činio nevidljivo vidljivim."* - Marko, vaš mentor

Nakon što ste doživjeli petkovnu production krizu i vaše buđenje potaknuto kavom, spremni ste za svoj prvi pravi zadatak. Ali prije nego što možete transformirati VelocityTech, trebate razumjeti **točno što transformirate**.

---

## 🎯 **Ciljevi Učenja za Tjedan 1**

Do kraja ovog tjedna, moći ćete:
- **IU1** - Objasniti osnovne DevOps principe i koncepte kulturalne transformacije
- **Mjeriti trenutno stanje** koristeći ključne DevOps metrike (DORA metrike)
- **Mapirati value stream-ove** za identifikaciju uskih grla i rasipanja
- **Dokumentirati problematične točke** s kvantificiranim utjecajem
- **Uspostaviti baseline** za praćenje transformacije

---

## 🎭 **Kontekst Priče za Ovaj Tjedan**

### **Realnost Ponedjeljak Ujutro**
Ponedjeljak je, 8:30. Stižete u VelocityTech sa svojom laptop-om, bilježnicom i novostečenom perspektivom sistemskog razmišljanja. Vikend je omogućio svima da se smirite, ali problemi od petka su još uvijek neriješeni.

**Vaša misija ovaj tjedan:** Postanite VelocityTech-ov dijagnostički specijalista. Dokumentirajte sve, izmjerite sve, razumijte sve.

---

## 📅 **Tjedni Raspored**

### **🎪 Scenarij Problematične Točke (15 minuta)**
#### *"Post-Mortem Ponedjeljak Ujutro"*

**[SCENA: VelocityTech konferencijska sala, 9:00. Fluorescentna svjetla zuje iznad. Mrlje od kave na stolu.]**

**MAJA** *(čitajući iz svojih bilježaka)*  
> "Dakle, petkova incident. Tri sata downtime-a, približno 50.000 pogođenih customer zapisa..."  
> "Klijent zahtijeva detaljan post-mortem do kraja tjedna."

**FILIP** *(trljajući umorne oči)*  
> "Proveo sam vikend ručno popravijući pokvarene user profile. Opet."  
> "Trebamo shvatiti zašto se ovo stalno događa."

**LUKA** *(obrambeno)*  
> "Moj kod radi dobro. Vjerojatno je problem s okruženjem."  
> "Možda kad bismo imali propisna testing okruženja..."

**ANA** *(preopterećena)*  
> "Ne mogu testirati sve. Previše je edge case-ova."  
> "A trebam validirati fix do srijede?"

**MARKO** *(gledajući vas)*  
> "Ovo je točno razlog zašto trebamo izmjeriti naše trenutno stanje."  
> "Ne možete popraviti ono što ne vidite."

**[Svi se okreću prema vama - novi praktikant s "svježom perspektivom"]**

**MAJA**  
> "Točno, ti si naš novi DevOps praktikant. Možeš li nam pomoći razumjeti što je pošlo po zlu?"

**[Ovo je vaš trenutak. Što vidite što oni ne vide?]**

---

### **📚 Sadržaj Predavanja (45 minuta)**

#### **1. Osnove DevOps-a i Kulturalna Transformacija (15 min)**

**Što je DevOps Stvarno?**
- **Nisu samo alati** - to je kulturalni pokret
- **CALMS Framework:**
  - **C**ulture - Suradnja umjesto silosa
  - **A**utomation - Eliminiraj ručni rad
  - **L**ean - Fokus na vrijednost, eliminiraj rasipanje
  - **M**easurement - Odluke temeljene na podacima
  - **S**haring - Dijeljenje znanja i odgovornosti

**Tri Puta DevOps-a:**
1. **Sistemsko Razmišljanje** - Optimiziraj cjelinu, ne dijelove
2. **Pojačaj Feedback Petlje** - Brza detekcija i oporavak
3. **Kultura Eksperimentiranja** - Učenje iz grešaka

#### **2. DORA Metrike - Četiri Ključa (15 min)**

**Lead Time za Promjene**
- Vrijeme od commit-a do produkcije
- VelocityTech trenutno: ~30 dana
- World-class target: <1 sat

**Deployment Frequency**
- Koliko često release-amo u produkciju
- VelocityTech trenutno: Mjesečno (s boli)
- World-class target: Na zahtjev

**Mean Time to Recovery (MTTR)**
- Koliko brzo popravljamo production probleme
- VelocityTech trenutno: 3-4 sata
- World-class target: <1 sat

**Change Failure Rate**
- Postotak deployment-a koji uzrokuju probleme
- VelocityTech trenutno: ~30%
- World-class target: 0-15%

#### **3. Osnove Value Stream Mapping-a (15 min)**

**Što je Value Stream Mapping?**
- Vizualna reprezentacija toka rada
- Identificira rasipanje i uska grla
- Pokazuje i trenutno i buduće stanje

**Tipovi Rada:**
- **Dodavanje vrijednosti** - Kupac plaća za ovo
- **Ne dodaje vrijednost ali potrebno** - Compliance, sigurnost
- **Čisto Rasipanje** - Čekanje, prerađivanje, predaja

**Uobičajeni Tipovi Rasipanja:**
- **Čekanje** - Kod čeka odobrenje/testiranje
- **Dodatna Obrada** - Ručni koraci koji bi mogli biti automatizirani
- **Defekti** - Bug-ovi pronađeni kasno u procesu
- **Transport** - Premještanje koda između okruženja

---

### **🧪 Laboratorijska Vježba: "Transformacijska Dijagnostika" (3 sata)**

#### **Dio 1: Baseline Procjena Metrika (45 min)**

**Vaš Zadatak:** Uspostaviti metrike trenutnog stanja za VelocityTech

**Potrebni Alati:**
- Excel/Google Sheets za prikupljanje podataka
- Pristup VelocityTech-ovom Jira/project management alatu
- Pristup Git repozitoriju
- Predlošci za intervjue

**Postupak Korak-po-Korak:**

1. **Analiza Lead Time-a**
   ```
   Pronađi zadnjih 10 završenih feature-a:
   - Datum zahtjeva za feature
   - Datum početka developmenta  
   - Datum završetka koda
   - Datum release-a u produkciju
   Izračunaj: Zahtjev → Produkcija vrijeme
   ```

2. **Praćenje Deployment Frequency**
   ```
   Pregledaj zadnjih 6 mjeseci:
   - Prebroji production deployment-e
   - Zabilježi deployment uspjeh/neuspjeh
   - Dokumentiraj deployment trajanje
   ```

3. **MTTR Kalkulacija**
   ```
   Pronađi zadnjih 10 production incidenata:
   - Vrijeme otkrivanja problema
   - Vrijeme početka rješavanja
   - Vrijeme rješavanja problema
   Izračunaj prosjek vremena oporavka
   ```

4. **Change Failure Rate**
   ```
   Zadnjih 20 deployment-a:
   - Uspješni deployment-i
   - Neuspješni/rolled back deployment-i
   - Hotfix-ovi u 24 sata
   Izračunaj postotak neuspjeha
   ```

#### **Dio 2: Value Stream Mapping Radionica (90 min)**

**Vaša Misija:** Mapiraj VelocityTech-ov trenutni development proces

**Proces za Mapiranje:** Od zahtjeva za feature do production deployment-a

**Intervjui sa Stakeholder-ima:**
- **5 min s vsakim karakterom** (Luka, Ana, Filip, Maja)
- Pitaj: "Prošetaj me kroz svoj tipični radni proces"
- Dokumentiraj: Korake, vremena čekanja, problematične točke

**Predložak za Mapiranje:**
```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│   Zahtjev   │────│ Development │────│  Testiranje │
│    za       │    │             │    │             │
│  Feature    │    │             │    │             │
└─────────────┘    └─────────────┘    └─────────────┘
  Čekanje: 3 dana    Proces: 10 dana   Čekanje: 2 dana
   
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│ Code Review │────│ Deployment  │────│ Production  │
│             │    │   Priprema  │    │  Monitoring │
└─────────────┘    └─────────────┘    └─────────────┘
  Čekanje: 2 dana    Proces: 4 dana   Čekanje: ???
```

**Identifikacija Rasipanja:**
- Označi **crveno** za čekanje/kašnjenja
- Označi **žuto** za potreban rad koji ne dodaje vrijednost  
- Označi **zeleno** za rad koji dodaje vrijednost

#### **Dio 3: Dokumentiranje Problematičnih Točaka (45 min)**

**Vaš Zadatak:** Stvori sveobuhvatan popis problematičnih točaka

**Kategorije za Dokumentiranje:**

1. **Tehničke Problematične Točke**
   - Nedosljednosti okruženja
   - Ručni deployment koraci
   - Uska grla testiranja
   - Praznine u monitoring-u

2. **Procesne Problematične Točke**
   - Kašnjenja odobrenja
   - Praznine u komunikaciji
   - Silosi znanja
   - Problemi dokumentacije

3. **Kulturalne Problematične Točke**
   - Kultura krivnje
   - Izbjegavanje rizika
   - Otpor promjenama
   - Nedostatak zajedničkih ciljeva

**Predložak Dokumentacije:**
```markdown
## Problematična Točka: [Naziv]
**Utjecaj:** Visok/Srednji/Nizak
**Učestalost:** Dnevno/Tjedno/Mjesečno  
**Pogađa:** [Koje članove tima]
**Trenutni Trošak:** [Vrijeme/Novac/Kvaliteta utjecaj]
**Korijenski Uzrok:** [Zašto se ovo događa]
**Potencijalno Rješenje:** [Pristup na visokoj razini]
```

---

## 🎭 **Razvoj Karaktera Ovaj Tjedan**

### **Evolucija Legacy Luke**
- **Ponedjeljak:** "Zašto radimo post-mortem? Bila je to samo loša sreća."
- **Srijeda:** "Pretpostavljam da bi bilo korisno znati zašto se moj kod ponaša drugačije..."
- **Petak:** "Možda mjerenje nije tako loša ideja."

### **Rast Anxious Ane**  
- **Ponedjeljak:** "Više dokumentacije? Već imam previše posla."
- **Srijeda:** "Ove metrike pokazuju da nisam usko grlo za koje sam mislila da jesam."
- **Petak:** "Ovi podaci bi mi mogli pomoći bolje prioritizirati testiranje."

### **Spoznaja Firefighter Filipa**
- **Ponedjeljak:** "Ne trebamo metrike, trebamo više monitoring alata."
- **Srijeda:** "Ovi uzorci... oni su predvidljivi."
- **Petak:** "Ako možemo predvidjeti probleme, možda ih možemo spriječiti."

### **Buđenje Manager Maje**
- **Ponedjeljak:** "Koliko će dugo trajati ova dijagnostika? Imamo rokove."
- **Srijeda:** "Ove metrike objašnjavaju zašto su naše procjene uvijek pogrešne."
- **Petak:** "Ovi podaci bi mi mogli pomoći dati realne obveze klijentima."

---

## 📊 **Kriteriji Ocjenjivanja**

### **Laboratorijski Rezultati (40 bodova)**
- **Izvještaj Baseline Metrika** (15 bodova)
  - Točan izračun DORA metrika
  - Jasna metodologija prikupljanja podataka
  - Pravilna statistička analiza

- **Value Stream Mapa** (15 bodova)
  - Kompletno end-to-end process mapiranje
  - Točna mjerenja vremena
  - Jasna identifikacija rasipanja

- **Popis Problematičnih Točaka** (10 bodova)
  - Sveobuhvata dokumentacija
  - Kvantificirane procjene utjecaja
  - Praktični uvidi

### **Unos u Character Story Journal (15 bodova)**
Napišite refleksiju od 500 riječi iz perspektive praktikanta:
- Što vas je najviše iznenadilo o trenutnom stanju VelocityTech-a?
- Koja karakterova perspektiva vam je pomogla bolje razumjeti probleme?
- Kako je vaš "sistemsko razmišljanje" moment s kavom utjecao na vašu analizu?
- Koje prilike za transformaciju vas najviše uzbuđuju?

---

## 🛠️ **Alati i Resursi Uvedeni**

### **Alati za Mjerenje**
- **Excel/Google Sheets** - Prikupljanje i analiza podataka
- **Jira/Azure DevOps** - Praćenje radnih stavki
- **Git analytics** - Metrike promjena koda
- **Stopwatch metodologija** - Mjerenje vremena procesa

### **Alati za Suradnju**
- **Microsoft Teams** - VelocityTech company kanali
- **Dijeljeni radni prostori** - Timska suradnja
- **Predlošci intervjua** - Razgovori sa stakeholder-ima
- **Standardi dokumentacije** - Dijeljenje znanja

---

## 🔄 **Retrospektivna Pitanja za Tjedan 1**

### **Što je Pošlo Dobro?**
- Koji uvidi o trenutnom stanju VelocityTech-a su vas iznenadili?
- Koje tehnike mjerenja su bile najrazjašnjavajuće?
- Kako su interakcije s karakterima poboljšale vaše razumijevanje?

### **Što je Bilo Izazovno?**
- Koje metrike su bile najteže točno prikupiti?
- S kakvim otporom ste se susreli od članova tima?
- Gdje su se vaše pretpostavke o problemima pokazale pogrešne?

### **Što Ćete Raditi Drugačije?**
- Kako biste poboljšali svoj dijagnostički pristup?
- Koji dodatni podaci bi bili vrijedni?
- Koji odnosi sa stakeholder-ima trebaju više razvoja?

---

## 🚀 **Pogled na Tjedan 2**

### **Pregled Sljedeće Krize**
Utorak ujutro, Luka stiže na posao i otkriva da je njegov najnoviji feature branch nestao. "Git konflikti," spominje Ana. "Izgubili smo 3 dana rada." Kriza version control-a počinje...

### **Vaša Sljedeća Misija**
Naoružani baseline metrikama i razumijevanjem problematičnih točaka, pomoći ćete VelocityTech-u implementirati pravilne Git workflow-e i prakse suradnje.

### **Vještine Koje Ćete Razviti**
- Git branching strategije
- Procesi code review-a  
- Workflow-i suradnje
- Rješavanje konflikata

---

## 📚 **Preporučeno Čitanje**

- **"The DevOps Handbook"** - Poglavlje 1-3 (Temeljni koncepti)
- **"Accelerate"** - Poglavlje 2 (DORA metrike istraživanje)
- **"Value Stream Mapping"** - Karen Martin (Poboljšanje procesa)
- **VelocityTech Interni Wiki** - Procesi i povijest tvrtke

---

*← [Nazad na Uvodnu Scenu](../story/opening-scene-hr.md) | [Dalje: Tjedan 2 Kriza Version Control-a →](week02-version-control-crisis-hr.md)*

---

> *"Mjerenje je prvi korak koji vodi kontroli i na kraju poboljšanju. Ako nešto ne možete izmjeriti, ne možete to razumjeti. Ako to ne možete razumjeti, ne možete to kontrolirati. Ako to ne možete kontrolirati, ne možete to poboljšati."* - H. James Harrington
