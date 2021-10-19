FROM python:3.9-slim-buster

WORKDIR /app

COPY requirements.txt .

RUN python3 -m pip install --upgrade pip && \
python -m pip install -r requirements.txt 

COPY . .

EXPOSE 5000

CMD ["gunicorn", "--workers=3", "--bind", "0.0.0.0:5000", "app:app"]
