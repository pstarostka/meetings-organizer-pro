FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["src/MOP.API/MOP.API.csproj", "src/MOP.API/"]
COPY ["src/MOP.Resolver/MOP.Resolver.csproj", "src/MOP.Resolver/"]
COPY ["src/MOP.Abstract/MOP.Abstract.csproj", "src/MOP.Abstract/"]
COPY ["src/MOP.Services/MOP.Services.csproj", "src/MOP.Services/"]
COPY ["src/MOP.DAL/MOP.DAL.csproj", "src/MOP.DAL/"]
RUN dotnet restore "src/MOP.API/MOP.API.csproj"
COPY . .
WORKDIR "/src/src/MOP.API"
RUN dotnet build "MOP.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "MOP.API.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MOP.API.dll"]