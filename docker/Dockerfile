FROM golang:1.14-alpine AS build

WORKDIR /src/
COPY main.go /src/
RUN CGO_ENABLED=0 go build -o /bin/demos

FROM alpine:3.7
COPY --from=build /bin/demos /bin/demos
RUN apk add vim apache2
#COPY --from=busybox:1.28 /bin/busybox /bin/busybox
ENTRYPOINT ["/bin/demos"]
#ENTRYPOINT ["/bin/busybox"]
