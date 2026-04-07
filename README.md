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

## .Env files

Copy the example env file and fill in real values (JWT and Cloudinary)

Backend:

```bash
cp snapmap-backend/.env.example snapmap-backend/.env
```

Edit `snapmap-backend/.env`. Compose overrides `MONGODB_URI` to use the local Mongo container, but you still need the other secrets.

Frontend (This can remain as it is):

```bash
cp snapmap-frontend/.env.example snapmap-frontend/.env
```

## Run everything

From this folder (the same folder as `docker-compose.yml`):

```bash
docker compose up --build
```

Then open:

| Service  | URL                   |
| -------- | --------------------- |
| Frontend | http://localhost:5173 |
| API      | http://localhost:8000 |
| MongoDB  | localhost:27017       |

Stop containers:

```bash
docker compose down
```

To wipe the database volume and start clean:

```bash
docker compose down -v
```

## Seed sample data

Run the following script once to fill database with sample data.

```bash
docker compose run --rm api python /app/src/app/utils/init/create_init_state.py
```

Logins to sample users:

| User Role | Email                     | Password         |
| --------- | ------------------------- | ---------------- |
| Admin     | adminuser@example.com     | adminuser123     |
| Moderator | moderatoruser@example.com | moderatoruser123 |
| Default   | testuser@example.com      | testuser123      |

Use this on a fresh database or after `docker compose down -v`.

## Repo layout

- `docker-compose.yml` — Frontend, Backend (API), and Mongo
- `snapmap-backend/` — Python API (submodule)
- `snapmap-frontend/` — React Web App (submodule)
