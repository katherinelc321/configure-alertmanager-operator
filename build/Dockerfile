FROM quay.io/app-sre/boilerplate:image-v2.3.2 AS builder

RUN mkdir -p /workdir
WORKDIR /workdir
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN make go-build

####
FROM registry.access.redhat.com/ubi8/ubi-minimal:latest

ENV USER_UID=1001 \
    USER_NAME=configure-alertmanager-operator

COPY --from=builder /workdir/build/_output/bin/* /usr/local/bin/

COPY build/bin /usr/local/bin
RUN  /usr/local/bin/user_setup

ENTRYPOINT ["/usr/local/bin/entrypoint"]

USER ${USER_UID}

LABEL io.openshift.managed.name="configure-alertmanager-operator" \
      io.openshift.managed.description="Operator to configure Alertmanager with PagerDuty and Dead Man's Snitch."
