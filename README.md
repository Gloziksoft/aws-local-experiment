# AWS S3 – Local Experiment

Praktický experiment so službou **Amazon S3** v lokálnom prostredí pomocou **AWS CLI, Dockeru a LocalStacku**.

## Čo je S3

**Amazon S3 (Simple Storage Service)** je objektové úložisko v AWS. Používa sa na ukladanie súborov a objektov, napríklad dokumentov, obrázkov, záloh, logov alebo build artefaktov.

Základné pojmy:

- **Bucket** – kontajner, v ktorom sú uložené objekty.
- **Object** – samotný uložený súbor.
- **Object key** – názov/cesta objektu v buckete.

S3 nie je relačná databáza.

## Prostredie

- Linux
- Docker
- Docker Compose
- LocalStack
- AWS CLI v2

LocalStack simuluje AWS služby lokálne, takže experiment neprebieha v reálnom AWS účte.

## Architektúra

```text
AWS CLI
   ↓
S3 API
   ↓
LocalStack :4566
   ↓
my-first-bucket
   ↓
test.txt
```

Parameter:

```bash
--endpoint-url=http://localhost:4566
```

hovorí AWS CLI, aby komunikovalo s lokálnym LocalStackom namiesto reálneho AWS.

## Spustenie LocalStack

```bash
docker compose up -d
```

Kontrola:

```bash
docker compose ps
```

LocalStack bol úspešne spustený v stave:

```text
aws-localstack   localstack/localstack:latest   Up ... (healthy)
```

LocalStack API sme overili:

```bash
curl http://localhost:4566/_localstack/info
```

Výsledok potvrdil aktívny LocalStack:

```text
"is_license_activated": true
```

## AWS CLI credentials

Pre lokálny experiment používame testovacie credentials:

```bash
export AWS_ACCESS_KEY_ID=test
export AWS_SECRET_ACCESS_KEY=test
export AWS_DEFAULT_REGION=eu-central-1
```

Nejde o skutočné AWS credentials.

## Kontrola S3

Najprv sme skontrolovali existujúce buckety:

```bash
aws --endpoint-url=http://localhost:4566 s3 ls
```

Zoznam bol prázdny, pretože ešte neexistoval žiadny bucket.

## Vytvorenie S3 bucketu

Vytvorili sme prvý bucket:

```bash
aws --endpoint-url=http://localhost:4566   s3 mb s3://my-first-bucket
```

Výsledok:

```text
make_bucket: my-first-bucket
```

Vytvorený bucket:

```text
my-first-bucket
```

## Vytvorenie testovacieho súboru

V Linuxe sme vytvorili testovací súbor:

```bash
echo "Hello AWS S3" > test.txt
```

Vznikol súbor:

```text
test.txt
```

## Upload súboru do S3

Súbor sme nahrali do bucketu:

```bash
aws --endpoint-url=http://localhost:4566   s3 cp test.txt s3://my-first-bucket/
```

Výsledok:

```text
upload: ./test.txt to s3://my-first-bucket/test.txt
```

Tým vznikol S3 objekt:

```text
my-first-bucket
└── test.txt
```

## Kontrola objektu

Obsah bucketu sme skontrolovali:

```bash
aws --endpoint-url=http://localhost:4566   s3 ls s3://my-first-bucket/
```

Výsledok:

```text
13 test.txt
```

To potvrdilo, že objekt `test.txt` bol úspešne uložený v buckete `my-first-bucket`.

## Čo som si prakticky overil

- čo je Amazon S3
- rozdiel medzi bucketom a objektom
- komunikáciu so S3 cez AWS CLI
- vytvorenie S3 bucketu
- vytvorenie objektu
- upload súboru do S3
- kontrolu objektov v buckete
- použitie LocalStacku ako lokálnej simulácie AWS
- použitie Docker Compose na spustenie LocalStacku
- použitie `--endpoint-url` na komunikáciu s lokálnym AWS API

## LocalStack vs. reálne AWS

Lokálny experiment:

```text
AWS CLI
   ↓
localhost:4566
   ↓
LocalStack
   ↓
lokálny S3
```

Reálne AWS:

```text
AWS CLI
   ↓
AWS API
   ↓
Amazon S3
   ↓
AWS Cloud
```

LocalStack používame na učenie, vývoj a testovanie. Experiment nevytvára reálny AWS S3 bucket a nevytvára AWS cloudové náklady.

## Stav experimentu

```text
Docker              OK
Docker Compose      OK
LocalStack          OK
AWS CLI             OK
S3 API              OK
S3 Bucket           OK
S3 Object           OK
Upload              OK
```

Bucket:

```text
my-first-bucket
```

Object:

```text
test.txt
```

## Ďalší plán

```text
S3
 ↓
IAM
 ↓
EC2
 ↓
RDS
 ↓
CloudWatch
```

Cieľom nie je naučiť sa celé AWS, ale prakticky pochopiť základné AWS služby, ich účel a použitie v backendovej/cloud architektúre.
