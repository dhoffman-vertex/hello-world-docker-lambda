# hello-world-docker-lambda

## Basic Operation

### Building Docker Container

```bash
docker build -t lambda/hello-world .
```

### Running the Docker Container

```bash
docker run --rm -p 9000:8080 lambda/hello-world
```

### Testing the Docker Container

```bash
curl -XPOST "http://localhost:9000/2015-03-31/functions/function/invocations" -d '{}'   
```

## Runtime Interface Emulator (RIE) for Python

### Installing locally

```bash
mkdir -p ~/.aws-lambda-rie
curl -Lo ~/.aws-lambda-rie/aws-lambda-rie https://github.com/aws/aws-lambda-runtime-interface-emulator/releases/latest/download/aws-lambda-rie
chmod +x ~/.aws-lambda-rie/aws-lambda-rie
```

### Updating the Dockerfile

Remove the line from Phase 3 that installs the RIE

```Dockerfile
ADD https://github.com/aws/aws-lambda-runtime-interface-emulator/releases/latest/download/aws-lambda-rie /usr/bin/aws-lambda-rie
```

### Running the Docker Container

```bash
docker run -d --rm -v ~/.aws-lambda-rie:/aws-lambda -p 9000:8080 --entrypoint /aws-lambda/aws-lambda-rie lambda/hello-world /entry.sh app.handler
```

### Testing the Docker Container

```bash
curl -XPOST "http://localhost:9000/2015-03-31/functions/function/invocations" -d '{}'   
```

## References

https://aws.amazon.com/blogs/aws/new-for-aws-lambda-container-image-support/