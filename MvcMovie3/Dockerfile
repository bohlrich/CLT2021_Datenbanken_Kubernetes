FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY [".", "MvcMovie/"]
RUN dotnet restore "MvcMovie/MvcMovie.csproj"
WORKDIR "/src/MvcMovie"
RUN dotnet build "MvcMovie.csproj" -c Release -o /app/build
RUN dotnet tool install --global dotnet-ef
RUN /root/.dotnet/tools/dotnet-ef migrations add InitialCreate && \
    /root/.dotnet/tools/dotnet-ef database update

FROM build AS publish
RUN dotnet publish "MvcMovie.csproj" -c Release -o /app/publish

FROM base AS final

ENV LANG de_DE.UTF-8
ENV LANGUAGE de_DE:de
ENV LC_ALL de_DE.UTF-8

WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MvcMovie.dll"]
