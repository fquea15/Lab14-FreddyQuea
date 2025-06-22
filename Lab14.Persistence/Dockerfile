FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["Lab14.Persistence/Lab14.Persistence.csproj", "Lab14.Persistence/"]
COPY ["Lab14.Application/Lab14.Application.csproj", "Lab14.Application/"]
COPY ["Lab14.Domain/Lab14.Domain.csproj", "Lab14.Domain/"]
COPY ["Lab14.Infrastructure/Lab14.Infrastructure.csproj", "Lab14.Infrastructure/"]
RUN dotnet restore "Lab14.Persistence/Lab14.Persistence.csproj"
COPY . .
WORKDIR "/src/Lab14.Persistence"
RUN dotnet build "./Lab14.Persistence.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./Lab14.Persistence.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Lab14.Persistence.dll"]
