FROM python:3.9-slim

WORKDIR /app

ENV PYTHONUNBUFFERED=1

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

RUN useradd -m appuser

COPY . .
RUN chown -R appuser:appuser /app

USER appuser

EXPOSE 5000

CMD ["python", "app.py"]
