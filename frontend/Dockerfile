# -----------------------------------------
# 1. Build stage (Node)
# -----------------------------------------
FROM node:20-alpine AS build

WORKDIR /app

# Copy package files
COPY package.json package-lock.json ./

# Install dependencies
RUN npm install

# Copy all source code
COPY . .

# Build React app
RUN npm run build


# -----------------------------------------
# 2. Production stage (Nginx)
# -----------------------------------------
FROM nginx:alpine

# Copy build files to Nginx html folder
COPY --from=build /app/dist /usr/share/nginx/html

# Copy custom nginx config (optional)
# Uncomment if you add one:
# COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
