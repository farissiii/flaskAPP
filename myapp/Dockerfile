# Dockerfile pour l'application Python Flask
FROM python:3.9-slim

WORKDIR /app

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

# --- ÉTAPE 1 : Correction des failles OS (OpenSSL, Libc, etc.) ---
# Met à jour les paquets système et nettoie le cache APT
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y --no-install-recommends \
    # Ajoute ici des dépendances OS supplémentaires si nécessaire (ex: build-essential)
    && apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# --- ÉTAPE 2 : Installation des dépendances Python de ton requirements.txt ---
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# --- ÉTAPE 3 : Correction forcée des failles Python spécifiques (Jaraco, Wheel) ---
# Ceci garantit que les versions vulnérables sont remplacées, même si requirements.txt en spécifie d'anciennes
RUN pip install --no-cache-dir --upgrade --force-reinstall \
    "jaraco.context>=6.1.0" \
    "wheel>=0.46.2"

COPY . .

# --- ÉTAPE 4 : Sécurité utilisateur ---
RUN adduser --disabled-password --gecos "" appuser
USER appuser

EXPOSE 5000

CMD ["python", "-m", "flask", "run", "--host=0.0.0.0", "--port=5000"]
