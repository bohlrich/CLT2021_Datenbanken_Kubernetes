FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY [".", "MvcMovie/"]
RUN dotnet restore "MvcMovie/MvcMovie.csproj"
WORKDIR "/src/MvcMovie"
RUN dotnet build "MvcMovie.csproj" -c Release -o /app/build
RUN dotnet tool install --global dotnet-ef
RUN /root/.dotnet/tools/dotnet-ef migrations add BuildMigration && \
    /root/.dotnet/tools/dotnet-ef database update

FROM build AS publish
RUN dotnet publish "MvcMovie.csproj" -c Release -o /app/publish

FROM base AS final

ENV LANG de_DE.UTF-8
ENV LANGUAGE de_DE:de
ENV LC_ALL de_DE.UTF-8
ENV ASPNETCORE_URLS=http://*:8080
ENV HTTP_PROXY=http://proxy.aspicon.lan:8080
ENV http_proxy=http://proxy.aspicon.lan:8080
ENV HTTPS_PROXY=http://proxy.aspicon.lan:8080
ENV https_proxy=http://proxy.aspicon.lan:8080
ENV NO_PROXY=localhost,127.0.0.1

WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MvcMovie.dll"]
