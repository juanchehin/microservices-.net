#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["Services/Ordening/Ordening.API/Ordering.API.csproj", "Services/Ordening/Ordening.API/"]
COPY ["Services/Ordening/Ordening.Infrastructure/Ordening.Infrastructure.csproj", "Services/Ordening/Ordening.Infrastructure/"]
COPY ["Services/Ordening/Ordening.Application/Ordering.Application.csproj", "Services/Ordening/Ordening.Application/"]
COPY ["Services/Ordening/Ordering.Domain/Ordering.Domain.csproj", "Services/Ordening/Ordering.Domain/"]
RUN dotnet restore "Services/Ordening/Ordening.API/Ordering.API.csproj"
COPY . .
WORKDIR "/src/Services/Ordening/Ordening.API"
RUN dotnet build "Ordering.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Ordering.API.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Ordering.API.dll"]
