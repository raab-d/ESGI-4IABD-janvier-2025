# 1. Base image
FROM python:3.11

# 2. Copy requirements.txt
COPY requirements.txt /app/requirements.txt

# 3. Set working directory
WORKDIR /app

# 4. Install python dependencies
RUN pip install --no-cache-dir -r requirements.txt

# 5. Copy app.py inside /app
COPY app.py /app

# 6. Expose port 9090
EXPOSE 9090

# 7. Maintainer (optionnel mais demandé dans le lab)
LABEL maintainer="ton_nom <ton_mail>"

# 8. Run command
CMD ["python", "app.py"]
