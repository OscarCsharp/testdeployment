FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["testdeploy/testdeploy.csproj", "testdeploy/"]
RUN dotnet restore "testdeploy/testdeploy.csproj"
COPY testdeploy/ testdeploy/
WORKDIR "/src/testdeploy"
RUN dotnet build "testdeploy.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "testdeploy.csproj" -c $BUILD_CONFIGURATION -o /var/lib/jenkins/dot-net-agent/workspace/testdeploy /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /var/lib/jenkins/dot-net-agent/workspace/testdeploy .
HEALTHCHECK --interval=30s --timeout=5s --start-period=5s \
  CMD curl --fail http://localhost:8080/health || exit 1
ENTRYPOINT ["dotnet", "testdeploy.dll"]
