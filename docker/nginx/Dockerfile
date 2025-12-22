FROM nginx:alpine

# Copiar configuración personalizada
COPY default.conf /etc/nginx/conf.d/default.conf

# Crear directorio para logs
RUN mkdir -p /var/log/nginx

EXPOSE 80 443
