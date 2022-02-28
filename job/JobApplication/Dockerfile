FROM mcr.microsoft.com/dotnet/aspnet:5.0-focal AS base
WORKDIR /app

RUN adduser -u 5678 --disabled-password --gecos "" appuser && chown -R appuser /app
USER appuser


FROM mcr.microsoft.com/dotnet/sdk:5.0-focal AS build
WORKDIR /src
COPY ["JobApplication.csproj", "./"]
run dotnet restore "JobApplication.csproj"
COPY . .
WORKDIR "/src/."
run dotnet build "JobApplication.csproj" -c Release -o /app/build


FROM build AS publish
RUN dotnet publish "JobApplication.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "JobApplication.dll"]
