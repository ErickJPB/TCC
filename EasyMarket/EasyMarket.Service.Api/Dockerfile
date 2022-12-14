#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["EasyMarket.Service.Api/EasyMarket.Service.Api.csproj", "EasyMarket.Service.Api/"]
COPY ["EasyMarket.Domain.Dto/EasyMarket.Domain.Dto.csproj", "EasyMarket.Domain.Dto/"]
COPY ["EasyMarket.Domain/EasyMarket.Domain.csproj", "EasyMarket.Domain/"]
COPY ["EasyMarket.Infra/EasyMarket.Infra.csproj", "EasyMarket.Infra/"]
COPY ["EasyMarket.Application/EasyMarket.Application.csproj", "EasyMarket.Application/"]
RUN dotnet restore "EasyMarket.Service.Api/EasyMarket.Service.Api.csproj"
COPY . .
WORKDIR "/src/EasyMarket.Service.Api"
RUN dotnet build "EasyMarket.Service.Api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "EasyMarket.Service.Api.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
CMD ASPNETCORE_URLS="http://*$PORT" dotnet EasyMarket.Service.Api.dll
