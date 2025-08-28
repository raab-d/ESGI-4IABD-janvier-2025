FROM python:3.11-slim

LABEL maintainer="samigrbj" version="1.0"

WORKDIR /app

COPY requirements.txt /app/requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

COPY app.py /app/

EXPOSE 9090

CMD ["python", "app.py"]
