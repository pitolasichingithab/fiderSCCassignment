# Stage 1: Build
FROM golang:1.25-alpine AS builder
WORKDIR /app
COPY . .
RUN go mod download
RUN go build -o fider-bin ./main.go

# Stage 2: Runtime (Staging Environment)
FROM alpine:latest
WORKDIR /root/
# Copy the binary from the builder stage
COPY --from=builder /app/fider-bin .
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/etc ./etc
COPY --from=builder /app/views ./views

EXPOSE 3000
CMD ["./fider-bin"]