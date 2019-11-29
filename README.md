
## Startup
The _first time_ you do this it'll build an image; and will take around 20 minutes.  Make sure you're connected to the internet, go get some tea
```
docker-compose up -d
```

## Login 
```
docker-compose exec scalaplay bash
cd /scripts

```


## Shutdown
When finished
```
docker-compose down
```


## Docker image rebuild
To rebuild this image 
```
docker-compose build
```
