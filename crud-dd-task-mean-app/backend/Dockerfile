# backend/Dockerfile
FROM node:18-alpine

# Create app directory
WORKDIR /usr/src/app

# Install dependencies
COPY package*.json ./
RUN npm install --production

# Copy source code
COPY . .

# Environment variables (adjust if your app uses different names)
ENV PORT=3000
ENV MONGO_URI=mongodb://mongo:27017/mean-crud-db

EXPOSE 3000

CMD ["npm", "start"]
