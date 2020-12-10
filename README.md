# Mountebank POC

## Objectives

Use mountebank (https://www.mbtest.org/) to proxy and record http requests to an external API (https://pokeapi.co)

## Config

Run mountebank with datadir parameter for recorded responses:

```sh
mkdir datadir

docker run --rm -p 2525:2525 -p 4545:4545 -v $PWD/datadir:/datadir andyrbell/mountebank:2.3.3 mb --datadir /datadir
```

Record imposter configuration

```sh
curl -i -X POST -H 'Content-Type: application/json' http://localhost:2525/imposters --data @proxy.json
```

## Make requests to target api

```sh
curl -i -X GET http://localhost:4545/api/v2/pokemon/ditto

curl -i -X GET http://localhost:4545/api/v2/ability/4

for type in {0..19}; do curl -i -X GET http://localhost:4545/api/v2/type/$type ; done
```

With the default proxy mode (proxyOnce), first request will be proxied and recorded, subsequent requests will be replayed by mountebank.
