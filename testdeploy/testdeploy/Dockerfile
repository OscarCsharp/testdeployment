# Use the ASP.NET runtime image for the base layer
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 5004

# Use the .NET SDK image for building the application
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# Copy the project file and restore dependencies
COPY testdeploy/testdeploy.csproj testdeploy/
RUN dotnet restore testdeploy/testdeploy.csproj

# Copy the rest of the project files
COPY testdeploy/
WORKDIR /src/testdeploy

# Build the application
RUN dotnet build testdeploy.csproj -c $BUILD_CONFIGURATION -o /app/build

# Publish the application
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish testdeploy.csproj -c $BUILD_CONFIGURATION -o /var/lib/jenkins/workspace/production/myskol-api /p:UseAppHost=false

# Define the final image
FROM base AS final
WORKDIR /app
COPY --from=publish /var/lib/jenkins/workspace/production/myskol-api .
HEALTHCHECK --interval=30s --timeout=5s --start-period=5s \
  CMD curl --fail http://localhost:5004/health || exit 1
ENTRYPOINT ["dotnet", "testdeploy.dll"]
