#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat 

#FROM mcr.microsoft.com/dotnet/framework/aspnet:4.8-windowsservercore-ltsc2019
#ARG source
#WORKDIR /inetpub/wwwroot
#COPY ./dotnetframework_docker/Published_Code/ .
#

FROM mcr.microsoft.com/dotnet/framework/sdk:4.8 AS build
WORKDIR /app

# copy csproj and restore as distinct layers
COPY *.sln .
COPY dotnetframework_docker/*.csproj ./aspnetapp/
COPY dotnetframework_docker/*.config ./aspnetapp/
RUN nuget restore

# copy everything else and build app
COPY dotnetframework_docker/. ./dotnetframework_docker/
WORKDIR /app/dotnetframework_docker
RUN msbuild /p:Configuration=Release


# copy build artifacts into runtime image
FROM microsoft/aspnet:4.7.2 AS runtime
WORKDIR /inetpub/wwwroot
COPY --from=build /app/dotnetframework_docker/. ./