# Use the official .NET SDK image for build
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY ["testdeploy/testdeploy.csproj", "testdeploy/"]
RUN dotnet restore "testdeploy/testdeploy.csproj"
COPY . .
WORKDIR "/src/testdeploy"
RUN dotnet build "testdeploy.csproj" -c Release -o /app/build

# Publish the application
FROM build AS publish
RUN dotnet publish "testdeploy.csproj" -c Release -o /app/publish

# Use the official .NET runtime image for the final stage
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
COPY --from=publish /app/publish .
EXPOSE 5004
ENTRYPOINT ["dotnet", "testdeploy.dll"]
