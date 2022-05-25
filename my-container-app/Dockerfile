#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["my-container-app/my-container-app.csproj", "my-container-app/"]
RUN dotnet restore "my-container-app/my-container-app.csproj"
COPY . .
WORKDIR "/src/my-container-app"
RUN dotnet build "my-container-app.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "my-container-app.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "my-container-app.dll"]