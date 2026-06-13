# FROM golang:1.22 AS builder
# WORKDIR /usr/src/app
# COPY . .
# RUN go mod download
# RUN go build -o product-catlog .
# FROM golang:1.22 AS release
# WORKDIR /usr/src/app
# COPY ./src/product-catalog/products/ ./products/
# COPY --from=builder /usr/src/app/product-catlog .
# ENV PRODUCT_CATALOG_PORT=8088
# EXPOSE ${PRODUCT_CATALOG_PORT}
# ENTRYPOINT ["./product-catlog"]

# ---------- Build Stage ----------
FROM golang:1.22 AS builder

WORKDIR /usr/src/app

# Copy dependency files first (enables caching)
COPY src/product-catalog/go.mod src/product-catalog/go.sum ./

# Download dependencies
RUN go mod download

# Copy full source code
COPY src/product-catalog/ .

# Build the binary
RUN go build -o product-catalog .

# ---------- Release Stage ----------
FROM alpine:3.20

WORKDIR /usr/src/app

# Copy binary from builder
COPY --from=builder /usr/src/app/product-catalog .

# Copy products folder (from builder to avoid path issues)
COPY --from=builder /usr/src/app/products ./products

# Set environment
ENV PRODUCT_CATALOG_PORT=8088

EXPOSE 8088

ENTRYPOINT ["./product-catalog"]
