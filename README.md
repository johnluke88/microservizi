# Microservizi
Progetto di riferimento basato sui concetti e le best practice di "Microservices in Action (2nd Edition)" di Manning. Questo repository contiene esempi/implementazioni di microservizi in Java con containerizzazione Docker.

> Nota: questo progetto si ispira al libro "Microservices in Action" per l'architettura e le pratiche; implementazioni, nomi e configurazioni qui sono adattabili al codice presente nel repository.

## Sommario
- Descrizione
- Obiettivi del progetto
- Architettura (panoramica)
- Tecnologie principali
- Come eseguire in locale
- Docker / Docker Compose
- Configurazione e variabili d'ambiente
- Osservabilità (logging, tracing, metrics)
- Test
- CI / CD suggerito
- Contributi
- Riferimenti e letture consigliate
- Licenza

## Descrizione
Questo repository contiene un set di microservizi realizzati in Java che mostrano principi pratici come:
- Design di servizi autonomi
- Comunicazione sincrona (REST) e asincrona (messaggistica)
- Gestione della configurazione centralizzata
- Resilienza, circuit breaker e retry
- Osservabilità: logging strutturato, metrics e tracing distribuito
- Deployment containerizzato

L'architettura e le decisioni di design si rifanno ai pattern e agli esempi descritti in "Microservices in Action (2nd Ed.)".

## Obiettivi del progetto
- Fornire esempi pratici e riutilizzabili di microservizi in Java.
- Mostrare come orchestrare build, test e deployment con Docker.
- Dimostrare pattern di resilienza, configurazione e osservabilità.

## Architettura (panoramica)
- API Gateway (se presente) — punto di ingresso per i client
- Service A — responsabilità: gestione entità X
- Service B — responsabilità: gestione entità Y
- Service Config — configurazione centralizzata (es. Spring Cloud Config)
- Message Broker — es. Kafka o RabbitMQ per comunicazioni asincrone
- Database per ogni servizio (pattern Database per servizio)

## Tecnologie principali
- Java 17 (o versione specifica del progetto)
- Spring Boot / Spring Cloud 
- Maven
- Docker / Docker Compose
- Message broker: Kafka
- Zipkin / OpenTelemetry per tracing
- Prometheus + Grafana per metrics

## Requisiti
- JDK 17+
- Maven 3.6+ (o Gradle)
- Docker 20+
- Docker Compose (se usato)
- (Opzionale) Kafka/RabbitMQ in locale o via Docker

## Come costruire (esempio con Maven)
Dalla root del repository:

- Build di tutti i moduli:
  mvn clean package

- Build e avvio di un singolo servizio (es. service-a):
  cd service-a
  mvn spring-boot:run
  oppure
  mvn -pl service-a -am spring-boot:run

Adatta i comandi in base alla struttura reale (Maven multimodulo, Gradle, ecc.).

## Eseguire con Docker / Docker Compose
Esempio (presuppone che siano presenti Dockerfile nei servizi e un docker-compose.yml):

- Costruisci le immagini:
  docker-compose build

- Avvia i servizi:
  docker-compose up

- Esegui in background:
  docker-compose up -d

- Ferma ed elimina i container:
  docker-compose down

Nel repository includere / aggiornare `docker-compose.yml` con servizi, reti e volumi necessari (DB, broker, config server, tracing, ecc.).

## Configurazione
- Usare Spring profiles (application.yml / application-{profile}.yml) per separare dev/prod.
- Variabili d'ambiente utili (esempi):
  - SPRING_PROFILES_ACTIVE=dev
  - SPRING_DATASOURCE_URL=jdbc:postgresql://db:5432/service_a
  - BROKER_URL=kafka:9092

Consigli: usare un Config Server o vault per segreti in produzione.

## Endpoints principali (esempio)
- Service A
  - GET /api/v1/items
  - POST /api/v1/items
- Service B
  - GET /api/v1/orders

(Aggiungere gli endpoint effettivi e eventuali esempi di request/response)

## Resilienza e pattern
- Circuit breaker: Resilience4j / Spring Cloud Circuit Breaker
- Retry e backoff
- Bulkhead per isolamento delle risorse

## Osservabilità
- Logging: JSON logging strutturato (es. Logback + encoder JSON)
- Tracing distribuito: OpenTelemetry / Zipkin
- Metrics: esposizione /actuator/prometheus per raccolta da Prometheus
- Health checks: /actuator/health

## Test
- Unit tests: JUnit 5 + Mockito
- Integration tests: testcontainers per DB e broker in locale
- Contract tests (opzionale): Pact / Spring Cloud Contract

## Struttura del repositor
- service-a/ — codice sorgente Service A
- service-b/ — codice sorgente Service B
- config-server/ — server di configurazione 
- docker-compose.yml
- docs/ — diagrammi e documentazione aggiuntiva

## Riferimenti e letture consigliate
- "Microservices in Action", Second Edition — Manning
- Documentazione Spring Boot / Spring Cloud
- Documentazione OpenTelemetry, Prometheus, Kafka / RabbitMQ
