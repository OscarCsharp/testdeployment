FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY ["testdeploy/testdeploy/testdeploy.csproj","testdeploy"]
RUN dotnet restore "testdeploy/testdeploy/testdeploy.csproj"
COPY .. 
WORKDIR "/src/testdeploy"
RUN dotnet build "testdeploy.csproj" -c Release -o /app/build


FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "testdeploy.csproj" -c  -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "testdeploy.dll"]
