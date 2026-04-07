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

This project supports 2 environments:

- `dev`
- `prod`

### Dev setup

```bash
cp snapmap-backend/.env.dev.example snapmap-backend/.env.dev
cp snapmap-frontend/.env.dev.example snapmap-frontend/.env.dev
```

`dev` uses:

- local MongoDB container
- local `./nsfw_model` folder
- Cloudinary folder: `snapmap-dev`
- backend dependencies: `requirements-dev.txt` (full set)

### Prod setup

```bash
cp snapmap-backend/.env.prod.example snapmap-backend/.env.prod
cp snapmap-frontend/.env.prod.example snapmap-frontend/.env.prod
```

`prod` uses:

- MongoDB Atlas URI from `.env.prod`
- NSFW checks via API (`NSFW_MODE=api`)
- Cloudinary folder: `snapmap-prod`
- backend dependencies: `requirements.txt` (minimal set)
- frontend runtime image: built static files on Nginx (no dev tooling at runtime)

### Where to get API tokens

- NSFW API token: [Hugging Face](https://huggingface.co/settings/tokens)
  (create a token with `Finegrained` scope, with `Make calls to Inference Providers` being checked)
- Cloudinary API token: [Cloudinary](https://console.cloudinary.com/app/settings/api-keys)
- MongoDB Atlas URI: [MongoDB Atlas](https://cloud.mongodb.com/)
- JWT Secret Generator: [JWT Secret Generator](https://jwtsecretkeygenerator.com/)

## Run with Docker Compose

Dev:

```bash
docker compose -f docker-compose.dev.yml up --build
```

Prod:

```bash
docker compose -f docker-compose.prod.yml up --build
```

Then open:

| Service  | URL                                                      |
| -------- | -------------------------------------------------------- |
| Frontend | [http://localhost:5173](http://localhost:5173)           |
| API      | [http://localhost:8000/docs](http://localhost:8000/docs) |
| MongoDB  | localhost:27017                                          |

Prod frontend runs on `http://localhost:4173`.

Stop containers:

```bash
docker compose -f docker-compose.dev.yml down
```

To wipe the database volume and start clean:

```bash
docker compose -f docker-compose.dev.yml down -v
```

## Seed sample data

Run the following script once to fill database with sample data.

```bash
docker compose -f docker-compose.dev.yml run --rm api python /app/src/app/utils/init/create_init_state.py
```

Logins to sample users:

| User Role | Email                                                         | Password         |
| --------- | ------------------------------------------------------------- | ---------------- |
| Admin     | [adminuser@example.com](mailto:adminuser@example.com)         | adminuser123     |
| Moderator | [moderatoruser@example.com](mailto:moderatoruser@example.com) | moderatoruser123 |
| Default   | [testuser@example.com](mailto:testuser@example.com)           | testuser123      |

Use this on a fresh database or after `docker compose down -v`.

## Repo layout

- `docker-compose.dev.yml` — Frontend, Backend (API), and local Mongo
- `docker-compose.prod.yml` — Frontend and Backend (Atlas Mongo via env)
- `snapmap-backend/` — Python API (submodule)
- `snapmap-frontend/` — React Web App (submodule)
