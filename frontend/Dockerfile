FROM node:18-alpine
WORKDIR /app/frontend

# Копируем файлы зависимостей
COPY package.json package-lock.json ./

# Устанавливаем зависимости
RUN npm install

# Копируем остальные файлы
COPY . .

# Собираем проект (если нужно)
# RUN npm run build

EXPOSE 5173
CMD ["npm", "run", "dev"]