FROM python:3.9-slim

WORKDIR /app
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

# Correction OS
RUN apt-get update && apt-get upgrade -y && \
    apt-get install -y --no-install-recommends && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

COPY requirements.txt .

# --- SOLUTION FORCEE ---
# 1. On installe les requirements
# 2. On supprime agressivement les dossiers de metadata problématiques
# 3. On réinstalle les versions fixes proprement
RUN pip install --no-cache-dir -r requirements.txt && \
    pip uninstall -y jaraco.context wheel || true && \
    pip install --no-cache-dir --upgrade --force-reinstall --ignore-installed \
    "jaraco.context>=6.1.0" \
    "wheel>=0.46.2" \
    "packaging>=24.0"

COPY . .

RUN adduser --disabled-password --gecos "" appuser
USER appuser
EXPOSE 5000
CMD ["python", "-m", "flask", "run", "--host=0.0.0.0", "--port=5000"]
