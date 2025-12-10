# Практична робота: Dockerfile та Docker Compose

## Мета
Навчитися працювати з Dockerfile, створити багатосервісний застосунок
(Flask + Redis) та запустити його за допомогою Docker Compose.

---

## Використані технології
- Python 3.9
- Flask
- Redis
- Docker
- Docker Compose

---

## Крок 1 — Створення базового Flask застосунку

Файл `app.py`:

```python
from flask import Flask
import redis, os

app = Flask(__name__)
redis_client = redis.StrictRedis(host='redis', port=6379, decode_responses=True)

@app.route('/')
def hello():
    count = redis_client.incr('hits')
    return f"Hello from Docker! This page has been viewed {count} times."

if __name__ == '__main__':
    port = int(os.environ.get('APP_PORT', 5000))
    app.run(host='0.0.0.0', port=port)
```

## Крок 2 — Файл requirements.txt

```
flask==3.1.0
redis==7.0.1
```

## Крок 3 — Dockerfile

```
FROM python:3.9

ENV PYTHONUNBUFFERED=1

WORKDIR /app

COPY requirements.txt .
COPY app.py .

RUN pip install -r requirements.txt

EXPOSE 5000

CMD ["python", "app.py"]
```

## Крок 4 — docker-compose.yml

```
services:
  web:
    build: .
    ports:
      - "5000:5000"
    environment:
      - APP_PORT=5000
    depends_on:
      - redis

  redis:
    image: redis:7
```

## Запуск проекту

```
docker compose up --build
```

Після запуску сервера відкрийте:
```
http://localhost:5000
```

## Зупинка сервісів

```
docker compose down
```

## Скріншоти

![1](/hello-docker-screen/1.jpg)


![2](/hello-docker-screen/2.jpg)


![3](/hello-docker-screen/3.jpg)


![4](/hello-docker-screen/4.jpg)


![5](/hello-docker-screen/5.jpg)

## Висновки

- У результаті виконання роботи я:

- створила та запустила Flask застосунок

- підключила Redis

- створила Dockerfile

- запустила застосунок за допомогою Docker Compose

- перевірила роботу сервісів у браузері
