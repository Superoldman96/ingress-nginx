ARG GOLANG_VERSION

FROM golang:${GOLANG_VERSION}-alpine3.22 AS builder
RUN mkdir /authsvc
WORKDIR /authsvc
COPY . ./
RUN CGO_ENABLED=0 GOOS=linux go build -o authsvc authsvc.go

FROM gcr.io/distroless/base-debian11
COPY --from=builder /authsvc/authsvc /
EXPOSE 8080
ENTRYPOINT ["/authsvc"]
