# Outerbase Studio

[![Deploy to Cloudflare](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/outerbase/studio)

**Outerbase Studio** is a lightweight, browser-based GUI for managing SQL databases, designed for simplicity and versatility. Initially built for LibSQL and SQLite, it now supports a broad range of databases, including:

**Supported Databases:**

- **SQLite-based Database**
  - Turso/LibSQL
  - SQLite (local files)
  - Cloudflare D1
  - rqlite
  - StarbaseDB
  - Val.town
- MySQL (beta, limited features)
- PostgreSQL (beta, limited features)

---

Give it a try directly from your browser

[![LibSQL Studio, sqlite online editor](https://github.com/user-attachments/assets/5d92ce58-9ce6-4cd7-9c65-4763d2d3b231)](https://libsqlstudio.com)
[![Libsql studio playground](https://github.com/user-attachments/assets/dcf7e246-fe72-4351-ab10-ae2d1658087d)](https://libsqlstudio.com/playground/client?template=chinook)

## Deploy to Docker instructions

Hello. First `git checkout master` because the develop branch is in active development.

At  `next.config.js` add
```
const nextConfig = {
  output: 'standalone',   <-- at this line
  ...
}
```

Then add Dockerfile
```
FROM node:20-alpine AS builder

# Setting working directory. All the path will be relative to WORKDIR
WORKDIR /app
# Installing dependencies
COPY package*.json ./
RUN npm install

# Copying source files
COPY . .

# Building app
RUN npm run build

# Copy only standalone server to new image
FROM node:20-alpine
WORKDIR /app
COPY --from=builder /app/.next/standalone ./
COPY --from=builder /app/public ./public
COPY --from=builder /app/.next/static ./.next/static
CMD ["node", "server.js"]
```

Then just build
```
docker build -t outerbase-studio .
```

and run
```
docker run -p 3000:3000 outerbase-studio
```

## Features

![libsqlstudio-git-preview (7)](https://github.com/user-attachments/assets/1d7a3d90-61e3-4a77-83a5-4bb096bbfb4b)

- **Query Editor**: It features a user-friendly query editor equipped with auto-completion and function hint tooltips. It allows you to execute multiple queries simultaneously and view their results efficiently.
- **Data Editor**: It comes with a powerful data editor, allowing you to stage all your changes and preview them before committing. The data table is highly optimized and lightweight, capable of rendering thousands of rows and columns efficiently.
- **Schema Editor**: It allows you to quickly create, modify, and remove table columns with just a few clicks without writing any SQL.
- **Connection Manager**: It includes a flexible connection manager, allowing you to store your connections locally in your browser. You can also store them on a server and share your connections across multiple devices.

The features mentioned above are just a few of the many we offer. Give it a try to explore everything we have in store
