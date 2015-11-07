製作 production image
=====================

建立 production images
----------------------

```
FROM {yourimage}
COPY ./ /nodjsSampleForDocker
WORKDIR /nodjsSampleForDocker

RUN /bin/bash -l -c 'npm i'
RUN /bin/bash -l -c 'node_modules/.bin/grunt prod'

ENV PORT "1337"
ENV NODE_ENV "production"
ENV DOMAIN_HOST "localhost:1337"

EXPOSE 1337
CMD /bin/bash -l -c 'npm start'

```

production 執行 image
---------------------

```
docker run -d \
--link nodejsSample_mysql \
--p 1337:1337 \
--restart always \
yourimage_prod
```
