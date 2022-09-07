############################
# STEP 1 build executable binary
############################
FROM golang:1.19-alpine AS builder



# Install git.
# Git is required for fetching the dependencies.

RUN apk update && apk add --no-cache git



WORKDIR /app
COPY src/go.mod src/hello.go ./

# Fetch dependencies.# Using go get.
RUN go get -d -v

RUN go mod download

# Build the binary.
RUN CGO_ENABLED=0 GOOS=linux GOARCH=arm64 go build -ldflags="-w -s" -o /go/bin/hello


############################
# STEP 2 build a small image
############################
FROM scratch

# Copy our static executable.
COPY --from=builder /go/bin/hello /go/bin/hello
# Run the hello binary.
#ENTRYPOINT ["/go/bin/hello"]
CMD ["/go/bin/hello"]

#FROM scratch
#COPY --from=builder /app/hello /app/
#ENTRYPOINT [ "/app/hello" ]