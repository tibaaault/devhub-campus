# Squelette à compléter — étape 3.
# Mêmes contraintes que les autres services (multi-stage, non-root, ≤ 200 Mo).

# ---- Stage 1 : build / install des dépendances ----
FROM python:3.12-slim AS build

WORKDIR /app

# TODO : copiez requirements.txt et installez les dépendances dans un venv ou un --user.
# Pourquoi ne PAS faire un simple `pip install` global dans cette stage ?
# (réponse dans RAPPORT.md)
RUN python -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"

# TODO : copiez le code de l'app.
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# ---- Stage 2 : runtime ----
FROM python:3.12-slim AS runtime

# TODO : LABEL org.opencontainers.image.source.
LABEL org.opencontainers.image.source="https://github.com/tibaaault/devhub-campus"

WORKDIR /app

# TODO : copiez depuis `build` le venv et le code, rien de plus.
COPY --from=build /opt/venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"

# TODO : créez l'utilisateur non-root, passez en USER.
COPY app/ ./app/
RUN useradd -m -u 1001 appuser
USER 1001

ENV PORT=8080 \
    LOG_LEVEL=info \
    PYTHONUNBUFFERED=1

EXPOSE 8080

# TODO : commande de démarrage (uvicorn app.main:app …).
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8080"]
