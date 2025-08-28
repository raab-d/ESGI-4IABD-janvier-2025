FROM python:3.9-slim

LABEL maintainer="ton_nom <ton_email>" version="1.0"

WORKDIR /app

COPY requirements.txt /app/requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

COPY app.py /app/

EXPOSE 9090

CMD ["python", "app.py"]
