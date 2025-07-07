FROM node:18-alpine
WORKDIR /app/backend

# Копируем файлы зависимостей
COPY package.json package-lock.json ./

# Устанавливаем все зависимости (включая devDependencies)
RUN npm install

# Копируем остальные файлы
COPY . .

# Собираем Strapi
RUN npm run build

EXPOSE 1337
CMD ["npm", "run", "develop"]