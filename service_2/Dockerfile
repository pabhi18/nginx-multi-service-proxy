FROM python:3.11-slim

RUN apt-get update && apt-get install -y curl

RUN pip install uv

WORKDIR /app

COPY pyproject.toml uv.lock ./

COPY . .

EXPOSE 8002

CMD ["uv", "run", "app.py"]
