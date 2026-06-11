# REST API — Go & Gin

A RESTful API built with Go and the Gin framework, using SQLite for storage and `golang-migrate` for database migrations.

## Tech Stack

- **Go 1.24.5**
- **Gin** — HTTP web framework
- **SQLite** — embedded database (`data.db`)
- **golang-migrate** — database schema migrations
- **Air** — live reload for development

## Project Structure

```
.
├── cmd/
│   ├── api/
│   │   └── main.go          # API server entrypoint
│   └── migrate/
│       ├── main.go          # Migration runner
│       └── migrations/      # SQL migration files
├── .air.toml                # Air live-reload config
├── go.mod
└── go.sum
```

## Prerequisites

- [Go 1.24+](https://golang.org/dl/)
- [Air](https://github.com/air-verse/air) (for live reload)
- [migrate CLI](https://github.com/golang-migrate/migrate) (for creating migrations)

Install Air:

```bash
go install github.com/air-verse/air@latest
```

Install the migrate CLI (with SQLite support):

```bash
go install -tags 'sqlite3' github.com/golang-migrate/migrate/v4/cmd/migrate@latest
```

Make sure `$(go env GOPATH)/bin` is in your `PATH`:

```bash
export PATH=$PATH:$(go env GOPATH)/bin
```

## Getting Started

**1. Clone the repository**

```bash
git clone <repository-url>
cd rest-api-go-gin
```

**2. Install dependencies**

```bash
go mod download
```

**3. Run database migrations**

```bash
# Apply all pending migrations
go run ./cmd/migrate up

# Roll back the last migration
go run ./cmd/migrate down
```

**4. Start the development server**

```bash
air
```

The server starts at `http://localhost:8080` and reloads automatically on file changes.

To run without live reload:

```bash
go run ./cmd/api
```

## Database Migrations

Migration files live in `cmd/migrate/migrations/` and follow the naming convention:

```
{version}_{description}.up.sql
{version}_{description}.down.sql
```

**Create a new migration:**

```bash
migrate create -ext sql -dir ./cmd/migrate/migrations -seq <migration_name>
```

**Apply migrations:**

```bash
go run ./cmd/migrate up
```

**Roll back migrations:**

```bash
go run ./cmd/migrate down
```

## Build

```bash
go build -o ./bin/api ./cmd/api
./bin/api
```
