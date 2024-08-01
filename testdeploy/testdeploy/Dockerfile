# Use the official .NET SDK image to build the application
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build

# Set the working directory
WORKDIR /app

# Copy the project files and restore dependencies
COPY testdeploy/testdeploy/*.csproj ./testdeploy/testdeploy/
RUN dotnet restore ./testdeploy/testdeploy/testdeploy.csproj

# Copy the rest of the files and build the project
COPY . ./
RUN dotnet build ./testdeploy/testdeploy/testdeploy.csproj -c Release -o /app/build

# Publish the application
RUN dotnet publish ./testdeploy/testdeploy/testdeploy.csproj -c Release -o /app/publish

# Use the official .NET runtime image for the final stage
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS runtime

# Set the working directory
WORKDIR /app

# Copy the published files from the build stage
COPY --from=build /app/publish .

# Define the entry point of the application
ENTRYPOINT ["dotnet", "testdeploy.dll"]
