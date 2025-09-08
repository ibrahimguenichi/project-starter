# =========================
# Makefile for ProLynk
# =========================

# Load environment variables from .env
include .env
export

# Default target
.PHONY: help
help:
	@echo ""
	@echo "Usage:"
	@echo "  make build             # Build Spring Boot Docker image"
	@echo "  make rebuild           # Rebuild (no cache) the app image"
	@echo "  make pull              # Pull latest images (Postgres, Mailpit, etc.)"
	@echo "  make up                # Start all containers (default: dev)"
	@echo "  make up-dev            # Start stack with dev profile"
	@echo "  make up-prod           # Start stack with prod profile"
	@echo "  make down              # Stop all containers"
	@echo "  make restart           # Restart all containers"
	@echo "  make ps                # Show running containers"
	@echo "  make logs              # Show logs of all containers"
	@echo "  make logs-postgres     # Show postgres logs"
	@echo "  make up-app            # Start only the Spring Boot app container"
	@echo "  make up-postgres       # Start only the Postgres container"
	@echo "  make up-mailpit        # Start only the Mailpit container"
	@echo "  make db-shell          # Open psql shell into Postgres DB"
	@echo "  make clean             # Remove all volumes & rebuild fresh"
	@echo ""

# -------------------------
# Build / Rebuild
# -------------------------
.PHONY: build
build:
	@echo "🔧 Building Spring Boot Docker image..."
	docker compose build app

.PHONY: rebuild
rebuild:
	@echo "♻️  Rebuilding Spring Boot Docker image (no cache)..."
	docker compose build --no-cache app

.PHONY: pull
pull:
	@echo "⬇️  Pulling latest Docker images..."
	docker compose pull

# -------------------------
# Start containers
# -------------------------
.PHONY: up
up: up-dev

.PHONY: up-dev
up-dev:
	@echo "🚀 Starting stack in DEV mode..."
	docker compose -f docker-compose.yml -f docker-compose.override.yml up -d

.PHONY: up-prod
up-prod:
	@echo "🚀 Starting stack in PROD mode..."
	docker compose -f docker-compose.yml up -d

.PHONY: down
down:
	@echo "🛑 Stopping all containers..."
	docker compose down

.PHONY: restart
restart: down up

.PHONY: ps
ps:
	@echo "📦 Showing running containers..."
	docker compose ps

# -------------------------
# Logs
# -------------------------
.PHONY: logs
logs:
	@echo "📄 Showing logs..."
	docker compose logs -f

.PHONY: logs-postgres
logs-postgres:
	@echo "📄 Showing only Postgres logs..."
	docker compose logs -f postgres

# -------------------------
# Start individual containers
# -------------------------
.PHONY: up-app
up-app:
	@echo "🚀 Starting Spring Boot app container..."
	docker compose up -d app

.PHONY: up-postgres
up-postgres:
	@echo "🚀 Starting Postgres container..."
	docker compose up -d postgres

.PHONY: up-mailpit
up-mailpit:
	@echo "🚀 Starting Mailpit container..."
	docker compose up -d mailpit

# -------------------------
# Connect to Postgres DB
# -------------------------
.PHONY: db-shell
db-shell:
	@echo "🐘 Connecting to Postgres database..."
	@if [ -z "$(POSTGRES_USER)" ] || [ -z "$(POSTGRES_DB)" ]; then \
		echo "❌ Missing POSTGRES_USER or POSTGRES_DB in .env"; \
	else \
		docker compose exec postgres psql -U $(POSTGRES_USER) -d $(POSTGRES_DB); \
	fi

# -------------------------
# Cleanup
# -------------------------
.PHONY: clean
clean:
	@echo "🔥 Removing containers, networks, and volumes..."
	docker compose down -v --remove-orphans
