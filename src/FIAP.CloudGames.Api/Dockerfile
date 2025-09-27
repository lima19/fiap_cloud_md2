# ===== Runtime base =====
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

# ===== Build =====
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Copiar apenas o csproj e restaurar dependências
COPY FIAP.CloudGames.Api.csproj ./
RUN dotnet restore FIAP.CloudGames.Api.csproj

# Copiar todo o restante do código
COPY . ./

# Build da aplicação
RUN dotnet build FIAP.CloudGames.Api.csproj -c Release -o /app/build

# ===== Publish =====
FROM build AS publish
RUN dotnet publish FIAP.CloudGames.Api.csproj -c Release -o /app/publish /p:UseAppHost=false

# ===== Final =====
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish ./
ENTRYPOINT ["dotnet", "FIAP.CloudGames.Api.dll"]
