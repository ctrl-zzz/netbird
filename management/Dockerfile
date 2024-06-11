FROM golang:1.21 AS builder
WORKDIR /usr/src/app
COPY go.mod go.sum .
RUN go mod download && go mod verify
COPY . .
ENV GOCACHE=/root/.cache/go-build
RUN --mount=type=cache,target="/root/.cache/go-build" cd management && CGO_ENABLED=1 go build .

FROM ubuntu:22.04
RUN apt update && apt install -y ca-certificates && rm -fr /var/cache/apt
ENTRYPOINT [ "/go/bin/netbird-mgmt","management"]
CMD ["--log-file", "console"]
COPY --from=builder /usr/src/app/management/management /go/bin/netbird-mgmt
