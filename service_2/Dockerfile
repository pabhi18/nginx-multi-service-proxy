FROM python:3.13-alpine

RUN apk update && apk add curl

RUN pip install uv

WORKDIR /app

COPY pyproject.toml uv.lock ./

COPY . .

EXPOSE 8002

CMD ["uv", "run", "app.py"]
