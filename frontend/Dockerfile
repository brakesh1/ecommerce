FROM node:20-alpine AS builder

ARG REACT_APP_BACKEND_URL
ARG REACT_APP_AWS_S3_BASE_URL

ENV REACT_APP_BACKEND_URL=$REACT_APP_BACKEND_URL
ENV REACT_APP_AWS_S3_BASE_URL=$REACT_APP_AWS_S3_BASE_URL
ENV NODE_ENV=production

WORKDIR /app

COPY package.json package-lock.json ./
RUN npm ci

COPY . .
RUN npm run build

FROM nginx:alpine

COPY --from=builder /app/build /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
