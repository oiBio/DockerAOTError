#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443



FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src

RUN dotnet workload install wasm-tools 

COPY ["DockerAOTTest2/Server/DockerAOTTest2.Server.csproj", "DockerAOTTest2/Server/"]
COPY ["DockerAOTTest2/Client/DockerAOTTest2.Client.csproj", "DockerAOTTest2/Client/"]
COPY ["DockerAOTTest2/Shared/DockerAOTTest2.Shared.csproj", "DockerAOTTest2/Shared/"]
RUN dotnet restore "DockerAOTTest2/Server/DockerAOTTest2.Server.csproj"
COPY . .
WORKDIR "/src/DockerAOTTest2/Server"
RUN dotnet build "DockerAOTTest2.Server.csproj" -c Release -o /app/build
FROM build AS publish
RUN dotnet publish "DockerAOTTest2.Server.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "DockerAOTTest2.Server.dll"]