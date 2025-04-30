# Arhitektura IT sustava u oblaku

## Opći podaci o predmetu

| **NAZIV KOLEGIJA**                               | IT sustavi u oblaku                                                                |
| ------------------------------------------------ | ---------------------------------------------------------------------------------- |
| **Studijski program**                            | Stručni diplomski studij Računarstvo, modul Programsko inženjerstvo                |
| **Nositelj kolegija**                            | Marin Kepec struč.spec.ing.techn.inf.                                              |
| **Suradnik na kolegiju**                         |                                                                                    |
| **Status kolegija**                              | Redovni                                                                            |
| **Godina**                                       | 1. godina (2. semestar)                                                            |
| **Bodovna vrijednost i oblik izvođenja nastave** | **ECTS koeficijent opterećenja studenata:** 6 <br> **Broj sati (P+V+S):** 15+30+15 |

## Opis kolegija
### Ciljevi kolegija
Student će steći praktična znanja i vještine projektiranja i implementacije arhitektura IT sustava u oblaku koristeći AWS platformu. Kroz kolegij, student će naučiti kako dizajnirati, implementirati i optimizirati svaki sloj arhitekture - od pohrane podataka, računalnih resursa i baza podataka do mrežne infrastrukture. Također će usvojiti principe AWS Well-Architected Frameworka, naučiti kako osigurati visoku dostupnost, skalabilnost i sigurnost sustava, implementirati automatizaciju infrastrukture kao koda te primijeniti najbolje prakse u dizajniranju cost-optimiziranih arhitektura. Student će steći kompetencije za evaluaciju i unaprjeđenje postojećih arhitektura te projektiranje novih rješenja prilagođenih specifičnim poslovnim zahtjevima.

### Očekivani ishodi učenja na razini kolegija (sa šifrom)
IU1 - Objasniti osnovne principe računarstva u oblaku i arhitekture sustava.  
IU2 - Planirati i implementirati pohranu i računske resurse u oblaku.  
IU3 - Projektirati skalabilne, dostupne i fleksibilne arhitekture.  
IU4 - Primijeniti sigurnosne prakse i kontrole u arhitekturama.  
IU5 - Upravljati i nadzirati troškove i performanse u AWS-u.  
IU6 - Automatizirati izgradnju infrastrukture.  
IU7 - Primijeniti AWS Well-Architected Framework za procjenu sustava.

### Sadržaj kolegija
Uvod u arhitekturu oblaka: upoznavanje s principima i najboljim praksama AWS arhitekture, AWS Well-Architected Framework, regionalna dostupnost i odabir AWS resursa. Osiguravanje pristupa: principi sigurnosti, IAM, upravljanje korisnicima, grupama i ulogama, sigurnosne politike i dozvole. Implementacija sloja za pohranu: Amazon S3, upravljanje bucket-ima i objektima, sigurnost podataka, versioning, životni ciklus podataka. Dodavanje računskog sloja: Amazon EC2, odabir instanci, Amazon Machine Images (AMI), opcije pohrane, konfiguracija korisničkih podataka, opcije plaćanja. Implementacija sloja baze podataka: Amazon RDS, DynamoDB, opcije baza podataka, sigurnost, backup i restore, replikacija. Kreiranje mrežnog okruženja: dizajn VPC-a, javne i privatne podmreže, pristupne točke, sigurnosne grupe, AWS kontrole pristupa. Povezivanje mreža: VPC peering, Transit Gateway, Site-to-Site VPN, Direct Connect. Sigurnost na razini korisnika, aplikacije i podataka: federacija korisnika, upravljanje više AWS računa, AWS Organizations, enkripcija podataka, AWS KMS. Implementacija nadzora, elastičnosti i visoke dostupnosti: CloudWatch, EventBridge, Auto Scaling, Elastic Load Balancing, Route 53. Automatizacija arhitekture: infrastruktura kao kod, AWS CloudFormation, AWS SAM. Implementacija keširanja: CloudFront, ElastiCache, strategije keširanja. Izgradnja nespregnutih arhitektura, serverless sustava i mikroservisa: Lambda, API Gateway, SQS, SNS, Step Functions, kontejnerske tehnologije. Primjena naučenih koncepata kroz projektni zadatak koji integrira sve aspekte dizajna arhitekture.

### Oblici izvođenja nastave po temama

| Tjedan | Tema predavanja (P)                      | Tema laboratorijskih vježbi (LV)                                                   | Ishodi učenja                     |
| ------ | ---------------------------------------- | ---------------------------------------------------------------------------------- | --------------------------------- |
| 1      | Uvod u predmet, arhitekturu oblaka i IAM | Ispitivanje IAM politika, Istraživanje AWS Identity and Access Management          | IU1, IU4, IU7                     |
| 2      | Sustavi za pohranu podataka u oblaku     | Kreiranje statičke web stranice za kafić s Amazon S3                               | IU2                               |
| 3      | Računalna infrastruktura u oblaku        | Odabir tipova instanci, Predstavljanje Amazon EFS                                  | IU2, IU5                          |
| 4      | Baze podataka u oblaku                   | Kreiranje Amazon RDS baze podataka                                                 | IU2                               |
| 5      | Mrežna infrastruktura u oblaku           | Odabir pravog tipa podmreže, Kreiranje virtualnog privatnog oblaka                 | IU3, IU4                          |
| 6      | Povezivanje mreža u oblaku               | Kreiranje VPC Peering veze, Ponavljanje i priprema za sigurnosne teme              | IU3, IU4                          |
| 7      | Sigurnost u oblaku                       | Osiguravanje aplikacija pomoću Amazon Cognito, Enkripcija podataka u mirovanju     | IU4                               |
| 8      | Nadzor i skalabilnost (1. dio)           | Kreiranje visoko dostupnog okruženja (1. dio)                                      | IU3, IU5                          |
| 9      | Nadzor i skalabilnost (2. dio)           | Kreiranje visoko dostupnog okruženja (2. dio)                                      | IU3, IU5                          |
| 10     | Automatizacija infrastrukture            | Automatizacija infrastrukture s AWS CloudFormation                                 | IU6                               |
| 11     | Keširanje i komunikacija među servisima  | Streaming dinamičkog sadržaja pomoću CloudFront, Izgradnja nespregnutih aplikacija | IU3, IU5                          |
| 12     | Serverless arhitekture (1. dio)          | Razdvajanje monolitne aplikacije s API Gateway                                     | IU3                               |
| 13     | Serverless arhitekture (2. dio)          | Implementacija serverless arhitekture na AWS-u, Uvod u projektni zadatak           | IU3                               |
| 14     |                                          | Rad na projektnom zadatku                                                          | IU1, IU2, IU3, IU4, IU5, IU6, IU7 |
| 15     |                                          | Finalizacija i prezentacija projektnog zadatka                                     | IU1, IU2, IU3, IU4, IU5, IU6, IU7 |

### Ocjenjivanje i vrednovanje rada studenata tijekom nastave i na ispitu

### Obavezna literatura

### Dopunska literatura
