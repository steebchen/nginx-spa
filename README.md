# NGINX Docker image for SPAs

[![docker pull steebchen/nginx-spa][image shield]][docker hub]

This docker image can be used for single page apps (SPAs) in history mode. It serves your index.html if no other file matches.

## Quickstart

To create your own dockerfile, simply copy your distribution folder (often `dist/` or `build/`) to `/app` into the image.

```Dockerfile
FROM steebchen/nginx-spa:stable

COPY dist/ /app

EXPOSE 80

CMD ["nginx"]
```

Simply run your container as follows:

```bash
docker build -t your-app .
docker run -p 8000:80 your-app
```

Now you can visit `http://localhost:8000/` in your browser or run `curl -v http://localhost:8000/` to check if it's working.

## What it does

Single page applications (SPAs) often use the [HTML5 history API][history api]. This results in neat urls like `/user/john`, but will result in a 404 when accessed directly in the browser.

This means you have to configure your webserver to serve your entrypoint (index.html) on all routes per default, which es exactly what this docker image does.

Additionally, routes containing a dot will default to a 404 to prevent sending the index.html for routes like `/static/asset.js`.
This is super useful, but may break your app if you have app urls with dots in it (which I doubt).

Files including a dot will also have a Cache-Control header set to instruct browsers to cache content for 30 days. This is perfect if you're using webpack or similar frameworks which usually makes use of creating files with hashes in their names, but if your static files will be just named "bundle.js" or "0.css" (index.html is fine of course) you should avoid this image.

## Supported tags and `dockerfile` links

- [`stable` (*dockerfile*)][stable] (recommended)
- [`latest` (*dockerfile*)][latest]

## Notes

- Gzip is enabled for html, xml, js and css
- A default public caching header is set
- The nginx daemon is turned off in the settings

[history api]: https://developer.mozilla.org/en-US/docs/Web/API/History_API
[latest]: https://github.com/steebchen/nginx-spa/blob/master/dockerfile
[stable]: https://github.com/steebchen/nginx-spa/blob/stable/dockerfile
[base image]: https://github.com/nginxinc/docker-nginx
[image shield]: https://img.shields.io/badge/dockerhub-steebchen%2Fnginx--spa-blue.svg
[docker hub]: https://registry.hub.docker.com/u/steebchen/nginx-spa/
