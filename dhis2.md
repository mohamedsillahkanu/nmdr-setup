docker pull dhis2/core:24.0.5
docker pull postgis/postgis:13-3.3

New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\dhis2-24"

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
      - dhis2-24-db:/var/lib/postgresql/data

  dhis2:
    image: dhis2/core:24.0.5
    ports:
      - "8092:8080"
    environment:
      DHIS2_DATABASE_HOST: db
      DHIS2_DATABASE_NAME: dhis2
      DHIS2_DATABASE_USERNAME: dhis
      DHIS2_DATABASE_PASSWORD: dhis
    depends_on:
      - db
    volumes:
      - dhis2-24-home:/opt/dhis2

volumes:
  dhis2-24-db:
  dhis2-24-home:
"@ | Out-File -FilePath "$env:USERPROFILE\dhis2-24\docker-compose.yml" -Encoding UTF8

cd "$env:USERPROFILE\dhis2-24"
docker compose up -d
