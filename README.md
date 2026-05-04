# Snapmap

Web-based photography community application with interactive map, designed for sharing and discovering geotagged photos.

## What's needed

- [Docker](https://docs.docker.com/get-docker/) with Compose

## Clone the repo

This project uses Git submodules for the app code.

```bash
git clone --recurse-submodules https://github.com/szemerics/snapmap
cd snapmap
```

If you have already cloned it without submodules, run:

```bash
git submodule update --init --recursive
```

## Environment setup

```bash
cp snapmap-backend/.env.example snapmap-backend/.env
cp snapmap-frontend/.env.example snapmap-frontend/.env
```

Fill in the values.

- **Backend:** `snapmap-backend/.env`. Dev Compose forces Mongo to the `mongo` container (your file can still say `localhost` for running the API outside Docker)
- **Frontend:** `snapmap-frontend/.env`. **`VITE_API_URL`** is the API address.

### Where to get API tokens

- NSFW API token: [Hugging Face](https://huggingface.co/settings/tokens)
  (create a token with `Finegrained` scope, with `Make calls to Inference Providers` being checked)
- Cloudinary API token: [Cloudinary](https://console.cloudinary.com/app/settings/api-keys)
- MongoDB Atlas URI: [MongoDB Atlas](https://cloud.mongodb.com/)
- JWT Secret Generator: [JWT Secret Generator](https://jwtsecretkeygenerator.com/)

## Docker

**Dev** (Vite + API + Mongo):

```bash
docker compose up --build
```

Open [http://localhost:5173](http://localhost:5173). API: [http://localhost:8000/docs](http://localhost:8000/docs).

**Prod** (built frontend + API, no Mongo in Compose):

```bash
docker compose -f docker-compose.prod.yml up --build
```

Open [http://localhost:4173](http://localhost:4173).

Make sure **`snapmap-frontend/.env`** has the right **`VITE_API_URL`** before `--build` (e.g. `http://localhost:8000` when the API is mapped to that port).

Stop:

```bash
docker compose down
```

Reset dev database volume:

```bash
docker compose down -v
```

## Seed sample data

After dev Compose is up:

```bash
docker compose run --rm api python /app/src/app/utils/init/create_init_state.py
```

Logins to sample users:

| User Role | Email                                                         | Password         |
| --------- | ------------------------------------------------------------- | ---------------- |
| Admin     | [adminuser@example.com](mailto:adminuser@example.com)         | adminuser123     |
| Moderator | [moderatoruser@example.com](mailto:moderatoruser@example.com) | moderatoruser123 |
| Default   | [testuser@example.com](mailto:testuser@example.com)           | testuser123      |

Use this on a fresh database or after `docker compose down -v`.

## Repo layout

- `docker-compose.yml` — dev
- `docker-compose.prod.yml` — prod
- `snapmap-backend/` — Python API (submodule)
- `snapmap-frontend/` — React web app (submodule)
