# Deploy to servers

The output of Quartz is just a folder: `public/`. Any web server can serve it. "Deploying" simply means **copying `public/` to the server's web folder**.

## Option A. Copy files with `rsync`

If you have SSH access to a server that already runs a web server (for example a university server):

```bash
npx quartz build
rsync -avz --delete public/ student@server.example.edu:/var/www/my-wiki/
```

- `-a`: keep the folder structure, `-v`: show what is copied, `-z`: compress during transfer.
- `--delete`: remove files on the server that no longer exist in `public/` (for deleted notes).
- The trailing `/` after `public/` means "copy the contents", not the folder itself.

Save it as a script so one command publishes everything:

```bash
#!/bin/bash
# deploy.sh
set -e
npx quartz build
rsync -avz --delete public/ student@server.example.edu:/var/www/my-wiki/
echo "Deployed!"
```

## Option B. Run the Docker image on the server

Build the image from [[3 Docker and nginx]], then on the server:

```bash
docker run -d --restart unless-stopped -p 80:80 my-wiki
```

`--restart unless-stopped` brings the site back after the server reboots.

## Option C. Hosting services

Cloudflare Pages, Netlify, and Vercel can build Quartz for you, like GitHub Pages. Use these settings:

- Build command: `npx quartz plugin install && npx quartz build`
- Output directory: `public`

## Which one should I choose?

| You want... | Use |
| --- | --- |
| A free public site with no server | [[2 GitHub Pages]] |
| A private site on campus or in your lab | [[3 Docker and nginx]] |
| To use an existing web server | Option A (`rsync`) |
