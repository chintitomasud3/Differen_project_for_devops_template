Masudur, তুমি যদি Laravel project বানানোর জন্য একটা Dockerfile তৈরি করতে চাও যেখানে **PHP + Composer আগে থেকেই install থাকবে**, তাহলে নিচের মতো একটা base Dockerfile ব্যবহার করতে পারো। এরপর container এ ঢুকে project setup করতে পারবে।  

---

## 🔹 Example Dockerfile (Laravel Ready)

```dockerfile
# Use official PHP image with extensions
FROM php:8.2-cli

# Install system dependencies
RUN apt-get update && apt-get install -y \
    unzip \
    git \
    curl \
    libzip-dev \
    && docker-php-ext-install zip pdo pdo_mysql

# Install Composer globally
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# Set working directory
WORKDIR /var/www

# Expose port for artisan serve
EXPOSE 8000

# Default command
CMD ["php", "-a"]
```

---

## 🔹 Workflow (Bangla-English)

1. **Dockerfile build করো**  
   ```bash
   docker build -t laravel-php .
   ```

2. **Container run করো**  
   ```bash
   docker run -it -p 8000:8000 -v ${PWD}:/var/www laravel-php bash
   ```

3. **Container এর ভিতরে Laravel project বানাও**  
   ```bash
   composer create-project laravel/laravel my-app
   cd my-app
   php artisan serve --host=0.0.0.0 --port=8000
   ```

👉 এখন browser এ গিয়ে `http://localhost:8000` খুললে Laravel project দেখতে পাবে।

---

## 🗂️ Diagram Workflow

```
Dockerfile → Build image → Run container → Composer create-project → php artisan serve
```

---

docker run -it -p 8000:8000 -v E:/docker_prac:/var/www laravel-php bash


composer create-project laravel/laravel my-app
cd my-app
php artisan serve --host=0.0.0.0 --port=8000

✅ এই setup এ PHP + Composer আগে থেকেই থাকবে। তুমি container এ ঢুকে project বানাতে পারবে।  

চাও কি আমি তোমাকে একটা **docker-compose.yml** ফাইলও বানিয়ে দিই, যাতে Laravel + MySQL একসাথে run হয় development environment এর জন্য?
