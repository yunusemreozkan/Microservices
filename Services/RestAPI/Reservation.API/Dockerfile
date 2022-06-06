#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS base
WORKDIR /app


FROM mcr.microsoft.com/dotnet/sdk:3.1 AS build
WORKDIR /src
COPY ["Services/RestAPI/Reservation.API/Reservation.API.csproj", "Services/RestAPI/Reservation.API/"]
COPY ["Services/RestAPI/Reservation.API.Services/Reservation.API.Services.csproj", "Services/RestAPI/Reservation.API.Services/"]
COPY ["Services/RestAPI/Reservation.API.Infrastructure/Reservation.API.Infrastructure.csproj", "Services/RestAPI/Reservation.API.Infrastructure/"]
COPY ["Services/RestAPI/Reservation.API.Models/Reservation.API.Models.csproj", "Services/RestAPI/Reservation.API.Models/"]
RUN dotnet restore "Services/RestAPI/Reservation.API/Reservation.API.csproj"
COPY . .
WORKDIR "/src/Services/RestAPI/Reservation.API"
RUN dotnet build "Reservation.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Reservation.API.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Reservation.API.dll"]