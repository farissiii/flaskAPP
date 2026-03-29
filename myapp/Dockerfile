FROM python:3.9-slim

WORKDIR /app

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

# --- ÉTAPE 1 : Correction des failles OS (OpenSSL, Libc, etc.) ---
# On ajoute 'apt-get upgrade' pour installer les patchs de sécurité
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y --no-install-recommends \
    # Ajoute ici des dépendances si nécessaire (ex: curl, build-essential)
    && apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# --- ÉTAPE 2 : Installation de tes dépendances ---
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# --- ÉTAPE 3 : Correction forcée des failles Python (Jaraco, Wheel) ---
# On force la mise à jour même si elles sont déjà dans requirements.txt
RUN pip install --no-cache-dir --upgrade "jaraco.context>=6.1.0" "wheel>=0.46.2"

COPY . .

# --- ÉTAPE 4 : Sécurité utilisateur ---
RUN adduser --disabled-password --gecos "" appuser
USER appuser

EXPOSE 5000

CMD ["python", "-m", "flask", "run", "--host=0.0.0.0", "--port=5000"]
