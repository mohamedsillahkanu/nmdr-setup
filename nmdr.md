# NMDR
docker pull mohamedsillahkanu/nmdr:3.0.1
docker run -d -p 8090:8080 --name nmdr mohamedsillahkanu/nmdr:3.0.1

# DHIS2 with PostgreSQL
@"
version: '3'
services:
  db:
    image: postgis/postgis:13-3.3
    environment:
      POSTGRES_DB: dhis2
      POSTGRES_USER: dhis
      POSTGRES_PASSWORD: dhis
    volumes:
      - dhis2-db:/var/lib/postgresql/data

  dhis2:
    image: dhis2/core:2.40.5
    ports:
      - "8091:8080"
    environment:
      DHIS2_DATABASE_HOST: db
      DHIS2_DATABASE_NAME: dhis2
      DHIS2_DATABASE_USERNAME: dhis
      DHIS2_DATABASE_PASSWORD: dhis
    depends_on:
      - db
    volumes:
      - dhis2-home:/opt/dhis2

volumes:
  dhis2-db:
  dhis2-home:
"@ | Out-File -FilePath "$env:USERPROFILE\dhis2\docker-compose.yml" -Encoding UTF8

cd "$env:USERPROFILE\dhis2"
docker compose up -d
