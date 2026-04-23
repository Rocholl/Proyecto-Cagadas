FROM node:20-alpine AS build

WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .

ARG VITE_BACKEND_URL
ENV VITE_BACKEND_URL=${VITE_BACKEND_URL}

RUN test -n "$VITE_BACKEND_URL" || (echo "ERROR: define VITE_BACKEND_URL (URL pública del API) para el build" >&2 && exit 1)

RUN npm run build

FROM node:20-alpine

WORKDIR /app

RUN npm install -g serve@14

COPY --from=build /app/dist ./dist

EXPOSE 5173

CMD ["serve", "-s", "dist", "-l", "5173"]
