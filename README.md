# Docker cheat sheet

Build the container image from the dockerfile

`docker build -t <tag name here> .`

Run the container with port binding

`docker run --rm -it -p 8080:80 <tag name here>`

List running containers

`docker container ls`

Stop running containers

`docker kill <container id>`

Get logs for a container

`docker logs <container id>`

# Alpine with aspnet core 3.1

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:3.1-alpine AS base

# Add some libs required by .NET runtime 
# https://github.com/dotnet/core/blob/master/Documentation/build-and-install-rhel6-prerequisites.md#troubleshooting
RUN apk add bash icu-libs krb5-libs libgcc libintl libssl1.1 libstdc++ zlib wget
RUN apk update

RUN wget https://dot.net/v1/dotnet-install.sh -O $HOME/dotnet-install.sh

RUN chmod +x $HOME/dotnet-install.sh
RUN $HOME/dotnet-install.sh -c 3.1

WORKDIR /app
EXPOSE 80
COPY . .
ENTRYPOINT ["dotnet", "MyWebApp.dll"]
```
