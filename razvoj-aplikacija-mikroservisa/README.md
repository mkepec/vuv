# Razvoj aplikacija i mikroservisa u oblaku

## Opći podaci o predmetu

| **NAZIV KOLEGIJA**                               | Razvoj aplikacija i mikroservisa u oblaku                                          |
| ------------------------------------------------ | ---------------------------------------------------------------------------------- |
| **Studijski program**                            | Stručni diplomski studij Računarstvo, modul Programsko inženjerstvo                |
| **Nositelj kolegija**                            | Marin Kepec struč.spec.ing.techn.inf.                                              |
| **Suradnik na kolegiju**                         |                                                                                    |
| **Status kolegija**                              | Redovni                                                                            |
| **Godina**                                       | 1. godina (1. semestar)                                                            |
| **Bodovna vrijednost i oblik izvođenja nastave** | **ECTS koeficijent opterećenja studenata:** 6 <br> **Broj sati (P+V+S):** 15+30+15 |


## Opis kolegija
### Ciljevi kolegija
Student će steći praktična znanja i vještine razvoja aplikacija i mikroservisa u cloud okruženju koristeći AWS platformu. Kroz kolegij, student će naučiti programski pristupati i upravljati cloud resursima, razbiti monolitnu aplikaciju na mikroservise, implementirati serverless arhitekturu, razviti sustave za pohranu i baze podataka, implementirati REST API-je, raditi s kontejnerima te uspostaviti kontinuiranu integraciju i kontinuiranu isporuku (CI/CD). Steći će kompetencije za dizajniranje, implementaciju i održavanje skalabilnih, visoko dostupnih i sigurnih cloud aplikacija.

### Očekivani ishodi učenja na razini kolegija (sa šifrom)
IU1: Objasniti principe razvoja aplikacija u oblaku koristeći AWS servise i razvojne alate.
IU2: Razviti aplikacije koje koriste servise za pohranu podataka, baze podataka i keširanje na AWS platformi.
IU3: Dizajnirati i implementirati REST API rješenja korištenjem API Gateway-a i serverless funkcija.
IU4: Izgraditi event-driven aplikacije primjenom AWS Lambda funkcija, SQS i SNS servisa.
IU5: Primijeniti kontejnerske tehnologije za razvoj, upravljanje i implementaciju aplikacija u oblaku.
IU6: Konfigurirati sigurnosne politike i upravljati pristupom te autentikacijom korisnika aplikacija.
IU7: Postaviti i primijeniti CI/CD procese za automatizaciju izgradnje, testiranja i implementacije aplikacija.

### Sadržaj kolegija
Uvod u razvoj aplikacija u oblaku: upoznavanje s cloud computing modelima, razvojnim ciklusom u cloud okruženju, uloge u cloud computingu i priprema AWS razvojnog okruženja (CLI, SDK, CloudShell, IDE). Razvoj rješenja za pohranu: korištenje Amazon S3, upravljanje bucket-ima i objektima, kontrola pristupa i sigurnost podataka. Osiguravanje pristupa cloud resursima: AWS Identity and Access Management (IAM), autentikacija, autorizacija, model podijeljene odgovornosti. Razvoj fleksibilnih NoSQL rješenja: rad s Amazon DynamoDB, particioniranje podataka, indeksi, operacije nad podacima, throughput, čitanje i pisanje. Razvoj REST API-ja: implementacija API-ja pomoću Amazon API Gateway, integracije, testiranje, optimizacija i sigurnost. Razvoj event-driven serverless rješenja: AWS Lambda funkcije, načini invokacije, upravljanje dozvolama, konfiguracija i monitoring. Uvod u kontejnere i kontejnerske servise: Docker, mikroservisi, AWS ECS, ECR, rad s kontejnerima. Keširanje informacija za skalabilnost: korištenje ElastiCache i CloudFront, strategije keširanja. Razvoj s messaging servisima: implementacija asinkrone komunikacije pomoću Amazon SQS i SNS, integracija u aplikacije. Definiranje workflowa za orkestraciju funkcija: korištenje AWS Step Functions, state machine tipovi, upravljanje kompleksnim tokovima rada. Razvoj sigurnih aplikacija na AWS-u: implementacija sigurnosnih mehanizama, autentikacija s AWS Cognito. Automatizacija implementacije pomoću CI/CD pipeline-a: DevOps principi, korištenje AWS Code servisa, infrastruktura kao kod, implementacija CI/CD procesa. Planiranje i implementacija projekta u oblaku: postavljanje, razvoj i prezentacija cjelovite mikroservisne aplikacije s CI/CD pipeline-om.

### Oblici izvođenja nastave po temama


| Tjedan | Tema predavanja (P)                                               | Tema laboratorijskih vježbi (LV)                                                                                | Tema konstrukcijskih vježbi (KV)              | Ishodi učenja                     |
| ------ | ----------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- | --------------------------------------------- | --------------------------------- |
| 1      | Dobrodošli u AWS Academy Cloud Developing, Uvod u razvoj na AWS-u | AWS dokumentacijska potraga, Istraživanje AWS CloudShell-a i razvojnog okruženja (IDE)                          |                                               | IU1                               |
| 2      | Razvoj rješenja za pohranu podataka                               | Rad s Amazon S3                                                                                                 |                                               | IU2                               |
| 3      | Osiguranje pristupa resursima u oblaku                            | Model podijeljene odgovornosti, Konfiguracija međuračunskog pristupa, Kreiranje IAM korisnika i grupa           |                                               | IU6                               |
| 4      | Razvoj fleksibilnih NoSQL rješenja                                | Izračunavanje RCU i WCU, Rad s Amazon DynamoDB                                                                  |                                               | IU2                               |
| 5      | Razvoj REST API-ja                                                | Razvoj REST API-ja s Amazon API Gateway                                                                         |                                               | IU3                               |
| 6      | Razvoj event-driven serverless rješenja                           | Korištenje AWS X-Ray s Lambda funkcijama, Kreiranje Lambda funkcija pomoću AWS SDK za Python                    |                                               | IU4                               |
| 7      | Uvod u kontejnere i kontejnerske servise                          | Migracija web aplikacije na Docker kontejnere, Izvođenje kontejnera na upravljanom servisu                      |                                               | IU5                               |
| 8      | Keširanje informacija za skalabilnost                             | Keširanje podataka aplikacije s ElastiCache, Implementacija CloudFront-a za keširanje i sigurnost aplikacije    |                                               | IU2                               |
| 9      | Razvoj rješenja s messaging servisima                             | Rad s Amazon messaging servisima, Implementacija sustava za razmjenu poruka koristeći Amazon SNS i Amazon SQS   |                                               | IU4                               |
| 10     | Definiranje workflowa za orkestraciju funkcija                    | Kreiranje jednostavnih kalkulatora pomoću Step Functions, Orkestracija serverless funkcija s AWS Step Functions |                                               | IU4                               |
| 11     | Razvoj sigurnih aplikacija na AWS-u                               | Implementacija autentikacije aplikacija koristeći Amazon Cognito                                                |                                               | IU6                               |
| 12     | Automatizacija implementacije pomoću CI/CD pipeline-a             | Automatizacija implementacije aplikacije koristeći CI/CD pipeline                                               | Planiranje projektnog zadatka u oblaku        | IU7                               |
| 13     |                                                                   |                                                                                                                 | Implementacija projekta u oblaku              | IU1, IU2, IU3, IU4, IU5, IU6, IU7 |
| 14     |                                                                   |                                                                                                                 | Implementacija projekta u oblaku              | IU1, IU2, IU3, IU4, IU5, IU6, IU7 |
| 15     |                                                                   |                                                                                                                 | Finalizacija i prezentacija projekta u oblaku | IU1, IU2, IU3, IU4, IU5, IU6, IU7 |

### Ocjenjivanje i vrednovanje rada studenata tijekom nastave i na ispitu

### Obavezna literatura

### Dopunska literatura
