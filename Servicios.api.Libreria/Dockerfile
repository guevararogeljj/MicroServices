FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["Servicios.api.Libreria/Servicios.api.Libreria.csproj", "Servicios.api.Libreria/"]
RUN dotnet restore "Servicios.api.Libreria/Servicios.api.Libreria.csproj"
COPY . .
WORKDIR "/src/Servicios.api.Libreria"
RUN dotnet build "./Servicios.api.Libreria.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./Servicios.api.Libreria.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Servicios.api.Libreria.dll"]
