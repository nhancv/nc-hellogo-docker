# nc-hellogo-docker
golang with docker (hello world)


## Docker: 
- https://www.tutorialspoint.com/docker/docker_overview.htm

## Golang:
- https://golang.org/
- https://github.com/docker-library/docs/tree/master/golang/

*hello.go*
```
package main
import "fmt"
func main() {
    fmt.Println("Hello World")
}
```

*Dockerfile*
```
FROM golang:1.8

WORKDIR /go/src/app
COPY . .

RUN go get -d -v ./...
RUN go install -v ./...

CMD ["app"]
```

*Docker command help*
```
docker run --help
```

*Build Dockerfile*: after build, an golang-image will created
```
docker build -t golang-image:0.1 .

#List images
docker images

#Remove image
docker rmi golang-image:0.1
```

*Run Dockerfile*: will create golang-container-tmp and  run CMD declared in Dockerfile, then remove container after finished.
```
docker run -it --rm --name golang-container-tmp golang-image:0.1
```

## Using docker-compose

*Noted*: 
You should put Dockerfile and docker-compose.yml in the same directory, so that the config build just like 
```
    build: .
```
If you need point to difference Dockerfile, config like
```
    build:
      context: .
      dockerfile: ./Dockerfile
```

*docker-compose.yml*
```
version: '3.7'
services:

  hello-golang:
    image: golang-image:0.1
    container_name: golang-container
    build: .
    command: go run hello.go
    volumes:
      - ./:/go/src/app
    working_dir: /go/src/app
```

### Run with `docker-compose run` 
auto remove container after finished, create container with name golang-container instance of golang-image:0.1 image, and run “hello-golang” service 
```
docker-compose run --rm hello-golang
```

### Run with `docker-compose up`
NOT remove container after finished, run “hello-golang” service 
```
docker-compose up

# Should call down to remove container to avoid conflict name
docker-compose down
```


