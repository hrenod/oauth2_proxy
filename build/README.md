oauth2_proxy build
==================

In order to build and push a new image:

1. Place the code under `app` directory on the same level as `docker-compose.yml`
2. Start golang container:
   `docker-compose up`
3. Attach to the container and run bash:
   `docker exec -it oauth2proxy_golang_1 bash`
4. (Optional) Skip installation of remote dependencies tests by commenting out the commands in `app/dist.sh``:
   `# dep ensure || exit 1`
   `#./test.sh`
5. Rebuild with local dependencies:
   `cp  providers/* /go/src/github.com/bitly/oauth2_proxy/providers/ && go install && rm -rf oauth2_proxy-2.2.1-alpha.linux-amd64.go1.10.3/ && rm -rf dist/* && ./dist.sh && tar xf ./dist/oauth2_proxy-2.2.1-alpha.linux-amd64.go1.10.3.tar.gz`
6. Exit container and copy the binary:
   `cp app/oauth2_proxy-2.2.1-alpha.linux-amd64.go1.10.3/oauth2_proxy-2.2.1-alpha.linux-amd64 ./`
7. Bake and push the new image:
   `docker-compose -f docker-compose-dist.yml build --no-cache && docker-compose -f docker-compose-dist.yml push`