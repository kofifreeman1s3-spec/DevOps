FROM node:18
WORKDIR /app
COPY . .
RUN npm install || echo "No dependencies"
CMD ["node", "index.js"]
