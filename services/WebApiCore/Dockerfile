from mcr.microsoft.com/dotnet/aspnet:5.0-focal AS base
WORKDIR /app
EXPOSE 80

ENV ASPNETCORE_URLS=http://+:80

RUN adduser -u 5678 --disabled-password --gecos "" appuser && chown -R appuser /app
USER appuser


FROM mcr.microsoft.com/dotnet/sdk:5.0-focal AS build
WORKDIR /src
COPY ["WebApiCore.csproj", "./"]
RUN dotnet restore "WebApiCore.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "WebApiCore.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "WebApiCore.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WebApiCore.dll"]
