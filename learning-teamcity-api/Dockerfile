#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["learning-teamcity-api/learning-teamcity-api.csproj", "learning-teamcity-api/"]
RUN dotnet restore "learning-teamcity-api/learning-teamcity-api.csproj"
COPY . .
WORKDIR "/src/learning-teamcity-api"
RUN dotnet build "learning-teamcity-api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "learning-teamcity-api.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "learning-teamcity-api.dll"]