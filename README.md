
## Startup
The _first time_ you do this it'll build an image; and will take around 20 minutes.  Make sure you're connected to the internet, go get some tea
```
docker-compose up -d
```
For more details on what is in the image, see [here](docker/README.md)

## Login 
```
docker-compose exec scala_machine bash
cd /scripts

```

## Developing in Scala
See guide [here](scripts/README.md)

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

## Docker remove image
If you never need this again
```
docker rmi twa/scala
```
