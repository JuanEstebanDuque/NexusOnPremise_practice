ARG NODE_IMAGE=node:22.12-alpine
ARG RUNTIME_IMAGE=nginx:1.27-alpine

FROM ${NODE_IMAGE} AS builder

WORKDIR /app

COPY package.json package-lock.json ./
RUN npm ci

COPY . .

ARG VITE_API_URL=/api

RUN npm run lint && VITE_API_URL=${VITE_API_URL} npm run build


FROM ${RUNTIME_IMAGE} AS prod

COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=builder /app/dist /usr/share/nginx/html

EXPOSE 80
