# --- build stage ---
FROM golang:1.21-alpine AS build
WORKDIR /src

COPY go.mod go.sum ./
RUN go mod download

COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o /out/backend .

# --- runtime stage ---
FROM alpine:3.19
RUN adduser -D -H appuser
WORKDIR /app
COPY --from=build /out/backend ./backend
USER appuser

EXPOSE 9000
ENTRYPOINT ["./backend"]
