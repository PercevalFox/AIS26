FROM alpine:3.20 AS builder

# Création user non-root
RUN addgroup -S app && adduser -S app -G app

# Copie du site web
COPY index.html /tmp/index.html

FROM nginx:alpine

# Copie config sécurisée
COPY nginx.conf /etc/nginx/nginx.conf
COPY security-headers.conf /etc/nginx/security-headers.conf
COPY --from=builder /tmp/index.html /usr/share/nginx/html/index.html

# Création des repo nécessaires
RUN mkdir -p /run/nginx /var/cache/nginx \
 && chown -R nginx:nginx /usr/share/nginx/html /run/nginx /var/cache/nginx \
 && chmod -R 755 /run/nginx /var/cache/nginx \
 # Nettoyage pour réduire la surface d’attaque
 && rm -rf /etc/nginx/conf.d/* /var/log/nginx/*

# Passage en non-root
USER nginx

# Port 
EXPOSE 8081

# auto monitoring)
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
 CMD curl -f http://127.0.0.1:8081/ || exit 1

VOLUME ["/usr/share/nginx/html"]

ENTRYPOINT ["nginx", "-g", "daemon off;"]
