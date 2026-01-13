# Dobrodošli u tvrtku

## Uvođenje novog zaposlenika - Mrežne operacije

**Datum:** Ponedjeljak, 8:47
**Od:** Ana, voditelj IT odjela
**Za:** Novi mrežni administrator
**Predmet:** HITNO - Dobrodošlica i neposredan zadatak

---

### Dobrodošli u tim (više-manje)

Prije svega, dobrodošli. Voljela bih da je ovo normalan prvi dan s kavom i upoznavanjem, ali smo u kriznom modu.

Naš prethodni mrežni administrator, "Stari Petar," dao je otkaz SMS porukom jutros u 6:23. Njegova poruka je glasila: *"Gotovo. Ne mogu se više baviti s ovim. Sretno."* Ne javlja se na telefon.

### Trenutna situacija

Naša mreža je potpuna crna kutija. Stvari se kvare, servisi padaju, a saznajemo tek kada:
- Diekroeu prestane raditi email
- Skladište ne može obrađivati narudžbe
- Korisnici počnu zvati podršku

100% smo **reaktivni**. Trebamo biti **proaktivni**.

Samo prošli tjedan:
- 3 prekida rada za koje nismo znali dok se korisnici nisu požalili
- 2 sata zastoja jer se "nešto" dogodilo s switchem
- 15.000 eura izgubljene zarade jer je skladište bilo offline

Izvršni tim postavlja pitanja na koja ne mogu odgovoriti:
- "Zašto nismo znali da je mreža pala?"
- "Što je uzrokovalo prekid rada?"
- "Kako spriječiti da se ovo ponovi?"

Trebam odgovore. Trebam vidljivost. Trebam nadzor mreže.

---

## "Dokumentacija" od prethodnika

Petar je ostavio točno JEDNU stvar na svom stolu: salvetu s mrljama od kave. To je to. Nema wikija. Nema dokumentacije. Nema Excel tablice s IP adresama. Samo ovo:

```
╔═══════════════════════════════════════════════════════╗
║       BILJEŠKE NA SALVETI (Petrov prijnos posla)      ║
╠═══════════════════════════════════════════════════════╣
║                                                       ║
║  GLAVNI SERVER: 192.168.3.?                           ║
║  Login: lab                                           ║
║  Password: password                                   ║
║                                                       ║
║  KLJUČ: Public_vtx                                    ║
║  (Tri puta je zaokružio crvenom olovkom)              ║
║                                                       ║
║  Dopisano na dnu:                                     ║
║  "Router = RB3011"                                    ║
║  "Core switch = CSS326"                               ║
║  "Stari switch = Cisco 2950 (sretno)"                 ║
║                                                       ║
╚═══════════════════════════════════════════════════════╝
```

To je sve što imamo.

---

## Vaša misija - Tjedan 1

Ne zanima me još fancy dashboard. Ne zanima me lijepa grafika.

**Trebam znati JEDNU stvar do petka:**

> **"Je li naša mrežna oprema gore ili dolje UPRAVO SADA?"**

To je to. Možete li mi napraviti sustav koji pokazuje "otkucaje srca" naše infrastrukture?

### Kako izgleda uspjeh

Do kraja ovog tjedna želim:
1. Vidjeti ekran koji mi pokazuje jesu li naši routeri, switchevi. računala, serveri živi
2. Znati je li nešto pokvareno PRIJE nego telefon zazvoni
3. Imati dokaz da možete nadzirati naš hardware

### Što imate

- IP adresu servera sa salvete (Petar je očito postavio Linux VM za "nešto")
- SNMP community string `Public_vtx` (što god to značilo)
- Pristup našem mrežnom hardwareu (MikroTik router, MikroTik switch, i prastarni Cisco switch)
- Vaš mozak
- Google
- **Ovaj laboratorijski vodič**

### Što NEMATE

- Dokumentaciju
- Plan B
- Petrov broj telefona (vjerujte mi, pokušala sam)

---

## Vremenski okvir i očekivanja

**Ovaj tjedan (Tjedan 1):**
- Instalirati infrastrukturu za nadzor
- Dobiti uvid u mrežne uređaje
- Potvrditi da se podaci prikupljaju

**Sljedeći tjedan (Tjedan 2):**
- Napraviti dashboardove tako da netehničke osobe mogu razumjeti status

**Tjedan 3:**
- Pripremiti za produkciju s upozorenjima i prilagođenim prikazima

---

## Još jedna stvar

Znam da je ovo puno za prvi dan. Ali iskreno? Ovo je točno onakav problem koji mrežni inženjeri rješavaju u stvarnom svijetu. Naslijediš nered, imaš nepotpune informacije, i moraš napraviti da radi.

Naučit ćete više u ova tri tjedna nego što biste u šest mjeseci normalnog rada.

Ako zaglavite, dokumentirajte što ste pokušali. Dobri inženjeri ne znaju sve, znaju kako saznati stvari.

**Pretvorimo ovaj kaos u jasnoću.**

Sretno,

**Ana**
Voditelj IT odjela
ana@firma.local
Lokalno: 2847

---

## Vaši prvi koraci

Idite na **Laboratorijski vodič za 1. tjedan** i počnite s "Misija 0: Pristup i izviđanje."

Zapamtite: Petar vam je ostavio IP adresu tog servera s razlogom. Vrijeme je da saznate što zapravo radi na njemu.

---

*P.S. - Ako uspješno pokrenete nadzor mreže, postoji budžet za pizzu za tim. Petar nikad nije ispunio to obećanje. Budite bolji od Petra.*
