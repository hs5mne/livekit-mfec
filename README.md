# Rama LiveKit Service Deployment

This repository contains the deployment configuration for the UAT LiveKit stack described in the deployment guide.

## Included files

- [.env.example](.env.example) — template for required environment variables
- [docker-compose.yaml](docker-compose.yaml) — Redis, LiveKit, and application containers
- [livekit.yaml](livekit.yaml) — LiveKit server configuration
- [redis.conf](redis.conf) — Redis configuration

## Required setup

1. Copy [.env.example](.env.example) to [.env](.env) and fill in production values.
2. Generate secrets for JWT and Redis if needed.
3. Ensure the firewall allows:
   - TCP 7880 for LiveKit signaling
   - TCP 8080 for the application API
   - UDP 50000-60000 for WebRTC/ICE
4. Start the stack:

```bash
docker compose -f docker-compose.yaml pull
docker compose -f docker-compose.yaml up -d
```

## Verification

Check the containers:

```bash
docker compose -f docker-compose.yaml ps
```

Application health check:

```bash
curl http://localhost:8080/health-check
```

Redis ping:

```bash
docker exec livekit-redis redis-cli -a "$REDIS_PASSWORD" --no-auth-warning ping
```

## Notes

- The stack uses host networking mode, matching the deployment guide.
- LiveKit and Redis are bound to 127.0.0.1 with the external IP setting enabled.
- The application service expects an image named in `HOSAPP_IMAGE` and a tag in `IMAGE_TAG`.
