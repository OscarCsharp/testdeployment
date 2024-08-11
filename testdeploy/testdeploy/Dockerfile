# Base image for running the application
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 5004

# Build image
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# Copy csproj file and restore dependencies
COPY ["testdeploy/testdeploy.csproj", "testdeploy/"]
RUN dotnet restore "testdeploy/testdeploy.csproj"

# Copy the entire project and build it
COPY testdeploy/. testdeploy/
WORKDIR /src/testdeploy
RUN dotnet build testdeploy.csproj -c $BUILD_CONFIGURATION -o /app/build

# Publish image
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish testdeploy.csproj -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Final image
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "testdeploy.dll"]
