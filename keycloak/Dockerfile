FROM quay.io/keycloak/keycloak:26.4.2 AS builder

ENV KC_HEALTH_ENABLED=true
ENV KC_METRICS_ENABLED=true
ENV KC_FEATURES_DISABLED=authorization,step-up-authentication,ciba
ENV KC_CACHE=local
ENV KC_TRANSACTION_XA_ENABLED=false

WORKDIR /opt/keycloak

COPY providers/ /opt/keycloak/providers/

RUN /opt/keycloak/bin/kc.sh build

FROM quay.io/keycloak/keycloak:26.4.2

COPY --from=builder /opt/keycloak/ /opt/keycloak/

USER root
RUN microdnf install -y curl && microdnf clean all
USER keycloak

EXPOSE 8080 9000

HEALTHCHECK --interval=15s --timeout=5s --retries=3 --start-period=40s \
  CMD curl -fsS http://localhost:9000/health/ready | grep -q '"status": "UP"' || exit 1
