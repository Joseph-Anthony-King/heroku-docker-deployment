#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
# For Heroku deployments we do not expose the ports as the Heroku dyno is reponsible for this...
#EXPOSE 80
#EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["HerokuDockerDeployment.csproj", "HerokuDockerDeployment/"]
RUN dotnet restore "HerokuDockerDeployment/HerokuDockerDeployment.csproj"
WORKDIR "/src/HerokuDockerDeployment"
COPY . .
WORKDIR "/src/HerokuDockerDeployment"
RUN dotnet build "HerokuDockerDeployment.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "HerokuDockerDeployment.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
# For Heroku deployments the Heroku dyno is responsible for assigning the port so we set the call as follows...
CMD ASPNETCORE_URLS=http://*:$PORT dotnet HerokuDockerDeployment.dll