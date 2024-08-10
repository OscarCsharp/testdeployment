# Dockerfile
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Adjust the path if necessary
COPY ["testdeploy/testdeploy/testdeploy.csproj", "testdeploy/"]

# Restore and build
RUN dotnet restore "testdeploy/testdeploy.csproj"
COPY . .
WORKDIR "/src/testdeploy"
RUN dotnet build "testdeploy.csproj" -c Release -o /app/build

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
COPY --from=build /app/build .

ENTRYPOINT ["dotnet", "testdeploy.dll"]
