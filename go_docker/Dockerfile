FROM golang:alpine AS builder

# Set necessary environmet variables needed for our image
ENV GO111MODULE=on \
    CGO_ENABLED=0 \
    GOOS=linux \
    GOARCH=amd64

# Move to working directory /build
WORKDIR /build

# had to add to original example
RUN go mod init docker 
RUN go mod tidy

# Copy and download dependency using go mod
COPY go.mod .
COPY go.sum .
RUN go mod download
# had to add to original example
RUN go get github.com/afdolriski/golang-docker/database

# Copy the code into the container
COPY . .

# Build the application
RUN go build -o main .

# Move to /dist directory as the place for resulting binary folder
WORKDIR /dist

# Copy binary from build to main folder
RUN cp /build/main .

# Build a small image
FROM scratch

COPY --from=builder /dist/main /
COPY ./database/data.json /database/data.json

# Command to run
ENTRYPOINT ["/main"]
