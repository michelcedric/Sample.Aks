#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0-buster-slim AS build
WORKDIR /src
COPY ["Sample.Aks.Api.csproj", "Sample.Aks.Api/"]
RUN dotnet restore "Sample.Aks.Api/Sample.Aks.Api.csproj"
WORKDIR "/src/Sample.Aks.Api"
COPY . .

RUN dotnet build "Sample.Aks.Api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Sample.Aks.Api.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Sample.Aks.Api.dll"]