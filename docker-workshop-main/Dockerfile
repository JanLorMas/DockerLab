# # Choose an appropriate base Node.js image
# FROM node:18

# # Set the working directory for the application
# WORKDIR /app

# # Copy package files for dependency installation
# COPY package.json package-lock.json ./

# # Install npm dependencies
# RUN npm install

# # Copy all project files from current directory to container
# COPY . .

# # Specify which port needs to be exposed
# EXPOSE 3000

# # Add the command to start the Next.js application
# CMD ["npm", "run", "dev"]


FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npx prisma generate
RUN npm run build
FROM node:20-alpine AS runner
WORKDIR /app
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/public ./public
COPY --from=builder /app/package.json ./
RUN npm install --only=production
EXPOSE 3000
ENV NODE_ENV=production
CMD npm start