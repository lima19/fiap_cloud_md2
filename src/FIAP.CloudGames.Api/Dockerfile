# Estágio Base para a imagem final
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

# Estágio de Build
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# Copia o .csproj e restaura as dependências primeiro para aproveitar o cache
COPY ["FIAP.CloudGames.Api/FIAP.CloudGames.Api.csproj", "FIAP.CloudGames.Api/"]
RUN dotnet restore "./FIAP.CloudGames.Api/FIAP.CloudGames.Api.csproj"

# Copia o resto do código fonte
COPY . .
WORKDIR "/src/FIAP.CloudGames.Api"
RUN dotnet build "./FIAP.CloudGames.Api.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Estágio de Publicação
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./FIAP.CloudGames.Api.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Estágio Final
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "FIAP.CloudGames.Api.dll"]