# Guard Dog - Lightweight Health Monitoring for Self-Hosted Infrastructure

Monitor services, hardware, and network connections from your Raspberry Pi or any server. CLI-driven, concurrent health checks, with email alerts and optional real-time dashboard.

## What It Does

- **Multi-target monitoring:** Docker containers, TCP/UDP services, bare-metal hardware
- **Concurrent health checks:** Goroutine-per-target polling with configurable intervals
- **Persistent state:** SQLite storage for target configuration and check history
- **Alerting:** SMTP email notifications when services go down or metrics breach thresholds
- **Real-time stream (v1.1):** gRPC streaming to a local web dashboard

## Quickstart

```bash
git clone git@github.com:NewJhez01/guard_dog.git
cd guard_dog

# 1. Build
go build -o guarddog ./cmd/guarddog

# 2. Configure (CLI, one-time)
./guarddog add docker --name gh-tracker --container github-tracker
./guarddog add tcp --name gdns --host localhost --port 53
./guarddog mail --setup

# 3. Run
./guarddog serve                    # dev: foreground, logs to stdout

# 4. Deploy (production)
docker compose up -d                # background, restart always

```

## Architecture

```

cmd/guarddog/
    main.go # Cobra-style CLI wiring
    add.go # Target registration
    serve.go # Start polling engine + gRPC server
    mail.go # Alert configuration

internal/
    cli/ # CLI parsing and validation
    tracker/
        poller.go # Concurrent health check orchestration
    checker/ # HTTP, TCP, UDP, Docker, hardware probes
    alert/
        evaluator.go # Threshold breach detection
        notifier.go # SMTP dispatch
    storage/
        sqlite.go # Target and state persistence
    stream/
        grpc.go # gRPC server for dashboard (v1.1)

```

## Stack

Go · SQLite · gRPC · Docker API · SMTP

Roadmap

- v1.0: CLI + polling + SQLite + email alerts
- v1.1: gRPC streaming + web dashboard
- v1.2: Hardware metrics (CPU, memory, temperature)
