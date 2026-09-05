# AWS Local Experiment

Praktický experiment s AWS službami v lokálnom prostredí pomocou LocalStack a Docker.

## Cieľ

Oboznámiť sa so základnými AWS službami a pochopiť ich použitie v backendovej/cloud architektúre.

## Služby

- S3
- IAM
- EC2
- RDS
- CloudWatch

## Prostredie

- Linux
- Docker
- Docker Compose
- LocalStack

## Poznámka

Experiment prebieha lokálne. Nejde o produkčnú AWS infraštruktúru.

Spravil si celý tok:

AWS CLI
   ↓
S3 API
   ↓
LocalStack :4566
   ↓
my-first-bucket
   ↓
test.txt

A výstup:

upload: ./test.txt to s3://my-first-bucket/test.txt

a následne:

13 test.txt

potvrdzuje, že súbor bol úspešne uložený do S3.

Tvoj README už správne definuje cieľ projektu ako praktické oboznámenie sa so základnými AWS službami v lokálnom prostredí pomocou LocalStacku a Dockeru.

Teraz by som spravil jednu dôležitú vec

Zapíšme tento prvý experiment do dokumentácie, aby si o pár mesiacov nemusel rozmýšľať, čo si vlastne robil.

Vytvor:

nano docs/02-s3.md

a vlož:

# AWS S3 – Local Experiment

## Cieľ

Vyskúšať základnú prácu s AWS S3 pomocou AWS CLI
v lokálnom prostredí LocalStack.

## Prostredie

- Linux
- Docker
- Docker Compose
- LocalStack
- AWS CLI v2

## S3

Amazon S3 (Simple Storage Service) je objektové úložisko,
ktoré sa používa na ukladanie súborov a objektov.

V tomto experimente používame lokálny LocalStack namiesto
reálneho AWS.

## Vytvorenie bucketu

```bash
aws --endpoint-url=http://localhost:4566 s3 mb s3://my-first-bucket

Výsledok:

make_bucket: my-first-bucket
Vytvorenie testovacieho súboru
echo "Hello AWS S3" > test.txt
Upload súboru
aws --endpoint-url=http://localhost:4566 \
  s3 cp test.txt s3://my-first-bucket/

Výsledok:

upload: ./test.txt to s3://my-first-bucket/test.txt
Kontrola obsahu bucketu
aws --endpoint-url=http://localhost:4566 \
  s3 ls s3://my-first-bucket/

Výsledok:

13 test.txt
Čo som si prakticky overil
vytvorenie S3 bucketu
komunikáciu s S3 cez AWS CLI
upload objektu do bucketu
kontrolu objektov v buckete
použitie LocalStack ako lokálnej AWS simulácie
Dôležité

Experiment prebieha lokálne cez LocalStack.
Nevytvára reálny AWS S3 bucket v AWS účte.


Potom:

```bash
git add docs/02-s3.md
git commit -m "Add S3 local experiment documentation"
git push
