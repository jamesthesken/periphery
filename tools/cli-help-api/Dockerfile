# Accept the Go version for the image to be set as a build argument.
ARG GO_VERSION=1.16

FROM golang:${GO_VERSION}-alpine

# Git is required for fetching the dependencies.
RUN apk update
RUN apk upgrade
RUN apk add --no-cache git
RUN apk add ca-certificates
RUN apk add libc6-compat
RUN apk add libgcc
RUN apk add libstdc++

# Set the working directory outside $GOPATH to enable the support for modules.
WORKDIR /src

# Fetch dependencies first; they are less susceptible to change on every build
# and will therefore be cached for speeding up the next build
COPY ./go.mod ./go.sum ./
RUN GO111MODULE=on go mod download

# Import the code from the context.
COPY ./ ./

# Build the executable to `/app`. Mark the build as statically linked.
RUN GO111MODULE=on CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build \
    -installsuffix 'static' \
    -o /app .

# Run the compiled binary.
ENTRYPOINT ["/app"]

