## Laravel Docker Prod
Es un proyecto generico para desplegar aplicaciones laravel 12 en servidores de produccion usando docker.

---

### Requisitos
- Docker
- Docker Compose

---

### Clonado
```bash
# 1. Clonar este repositorio
git clone https://github.com/KevinHGitCode/laravel-docker-prod.git

# 2. Entrar en el directorio del proyecto
cd laravel-docker-prod

# 3. Clonar tu aplicacion Laravel dentro de la carpeta src
git clone https://github.com/your-username/your-laravel-app.git src

# 4. Quitar el control de versiones (opcional)
rm -rf .git
rm -rf src/.git
# En windows usa:
rmdir /s /q .git
rmdir /s /q src\.git
```

---

### Comandos para desplegar
```bash
# 1. Construir y levantar contenedores
docker-compose up -d --build

# 2. Ejecutar migraciones (primera vez)
docker-compose exec app php artisan migrate --force

# 2.1 Ejecutar seeders (si es necesario)
docker-compose exec app php artisan db:seed --force

# 3. Ver logs
docker-compose logs -f

# 4. Reiniciar servicios
docker-compose restart
```

---

### Estructura de carpetas
```
laravel-docker-prod/
├── docker/
│   ├── nginx/
│   │   ├── Dockerfile
│   │   └── default.conf
│   ├── php/
│   │   ├── Dockerfile
│   │   └── php.ini
│   └── supervisor/
│       └── supervisord.conf
├── src/
│   └── [código Laravel aquí]
|   └── .env  ← Este es para Laravel
├── mysql-data/
│   └── .gitkeep
├── docker-compose.yml
├── .env ← Este es para Docker Compose
└── .dockerignore
```

---

### Notas
- Asegúrate de configurar correctamente el archivo `.env` tanto para Laravel como para Docker Compose.
- El contenedor de MySQL almacena los datos en la carpeta `mysql-data` para persistencia.
- Puedes personalizar la configuración de Nginx y PHP modificando los archivos en la carpeta `docker/nginx` y `docker/php` respectivamente.
- Recuerda ejecutar las migraciones con `--force` en producción para evitar confirmaciones interactivas.
- Este proyecto está diseñado para entornos de producción, asegúrate de seguir las mejores prácticas de seguridad y optimización para Laravel y Docker.
