# Learning ASP.Net Core Razor Pages

These are notes for learning Razor pages.

I have created a Dockerised version of the notes that runs as a website on an Alpine Linux image running an Apache web server.

## Dockerfile

```bash
    FROM httpd:alpine
    COPY ./html/ /usr/local/apache2/htdocs/
```

## Build

```bash
    docker build -t learning-razor-pages-app .
```

## Run

```bash
    docker run -d --name learning-razor-pages-web -p 8080:80 learning-razor-pages-app
```
