# Stage 1: Build
FROM --platform=${TARGETPLATFORM:-linux/amd64} golang:1.25-bookworm AS builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN mkdir -p /app/dist
RUN go build -o fider-bin ./main.go

# Stage 2: Runtime (Staging Environment)
FROM --platform=${TARGETPLATFORM:-linux/amd64} debian:bookworm-slim AS runtime
WORKDIR /root/
# Copy the binary from the builder stage
COPY --from=builder /app/fider-bin .
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/etc ./etc
COPY --from=builder /app/views ./views

EXPOSE 3000
CMD ["./fider-bin"]