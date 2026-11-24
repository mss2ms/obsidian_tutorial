# Docker and nginx

Sometimes you do not want a public website, for example a wiki only for your lab or the campus network. Then you can serve `public/` from your own machine with **nginx** (a web server) inside **Docker** (a container).

## Idea

```
public/ (built by Quartz)  ──COPY──►  nginx container  ──port 80──►  http://<server-ip>/
```

nginx only needs a folder of HTML files. That is exactly what `public/` is.

## Step 1. Build the site

```bash
npx quartz build
```

## Step 2. Write a Dockerfile

Create a file named `Dockerfile` (no extension) next to `public/`:

```dockerfile
FROM nginx:alpine
COPY public/ /usr/share/nginx/html/
```

- `FROM nginx:alpine`: start from a small image that already has nginx.
- `COPY public/ ...`: put your website where nginx looks for files.

## Step 3. Build and run

```bash
docker build -t my-wiki .
docker run -d --name my-wiki -p 80:80 my-wiki
```

- `-p 80:80`: port 80 on your machine → port 80 in the container.
- Open `http://localhost` (or `http://<server-ip>` from another computer on the same network).

## Step 4. Update after writing new notes

```bash
npx quartz build
docker build -t my-wiki .
docker stop my-wiki && docker rm my-wiki
docker run -d --name my-wiki -p 80:80 my-wiki
```

## Note on pretty URLs

Quartz links look like `/notes/my-note` (no `.html`). If such links return 404 in nginx, add a config that tries `.html` too:

```nginx
# nginx.conf
server {
  listen 80;
  root /usr/share/nginx/html;
  location / {
    try_files $uri $uri.html $uri/ =404;
  }
}
```

and add this line to the Dockerfile:

```dockerfile
COPY nginx.conf /etc/nginx/conf.d/default.conf
```
