# --- build stage ---
FROM golang:1.21-alpine AS build
WORKDIR /src

COPY go.* ./
RUN go mod download

COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o /out/frontend .

# --- runtime stage ---
FROM alpine:3.19
RUN adduser -D -H appuser
WORKDIR /app
COPY --from=build /out/frontend ./frontend
COPY --from=build /src/static ./static
COPY --from=build /src/templates ./templates
USER appuser

EXPOSE 8080
ENTRYPOINT ["./frontend"]
