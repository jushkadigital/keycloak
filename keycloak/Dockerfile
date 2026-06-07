FROM quay.io/keycloak/keycloak:26.4.2 AS builder

ENV KC_HEALTH_ENABLED=true
ENV KC_METRICS_ENABLED=true
ENV KC_FEATURES=hostname:v2
ENV KC_FEATURES_DISABLED=step-up-authentication,ciba
ENV KC_DB=postgres
ENV KC_CACHE=local
ENV KC_TRANSACTION_XA_ENABLED=false

WORKDIR /opt/keycloak

COPY --chown=keycloak:keycloak providers/ /opt/keycloak/providers/
COPY --chown=keycloak:keycloak themes/ /opt/keycloak/providers/

RUN touch -m --date=@1743465600 /opt/keycloak/providers/* && /opt/keycloak/bin/kc.sh build

FROM quay.io/keycloak/keycloak:26.4.2

COPY --from=builder /opt/keycloak/ /opt/keycloak/

ENV KC_LOG_LEVEL=INFO
