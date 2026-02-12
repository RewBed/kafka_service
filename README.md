# Kafka Service (Docker Compose)

## Что внутри
- `kafka` (`apache/kafka:4.1.1`, single-node, KRaft, без ZooKeeper)
- `kafka-ui` для просмотра топиков/сообщений

## Быстрый старт
```bash
docker compose up -d
```

Или через `make`:
```bash
make up
```

Если до этого запускался другой образ Kafka (например Bitnami), очистите volume перед первым запуском:
```bash
docker compose down -v
docker compose up -d
```

## Доступ
- Kafka external bootstrap: `${KAFKA_EXTERNAL_HOST}:${KAFKA_EXTERNAL_PORT}` (по умолчанию `localhost:9094`)
- Kafka UI: `http://localhost:8080`

## Аутентификация Kafka
Для внешнего listener включена аутентификация `SASL_PLAINTEXT` (механизм `PLAIN`).
Логин/пароль задаются в `.env`:
- `KAFKA_CLIENT_USER`
- `KAFKA_CLIENT_PASSWORD`
- `kafka-ui` подключается к внутреннему listener `kafka:9092` (внутри docker-сети), аутентификация на нем не включена.

Пример клиентских свойств:
```properties
security.protocol=SASL_PLAINTEXT
sasl.mechanism=PLAIN
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="app" password="change-me-strong-password";
```

## Аутентификация Kafka UI
На веб-интерфейсе включена форма логина (`LOGIN_FORM`).
Учетные данные задаются в `.env`:
- `KAFKA_UI_USERNAME`
- `KAFKA_UI_PASSWORD`

## Проверка
```bash
docker compose ps
docker compose logs -f kafka
```

## Полезные команды
```bash
make logs
make down
make restart
```

## Настройки
Параметры находятся в `.env`:
- `COMPOSE_PROJECT_NAME`
- `KAFKA_EXTERNAL_PORT`
- `KAFKA_UI_PORT`
- `KAFKA_EXTERNAL_HOST`
- `KAFKA_CLUSTER_ID`
- `KAFKA_CLIENT_USER`
- `KAFKA_CLIENT_PASSWORD`
- `KAFKA_UI_USERNAME`
- `KAFKA_UI_PASSWORD`
