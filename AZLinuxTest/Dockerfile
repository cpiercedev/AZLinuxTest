FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build

# Install Node.js
RUN curl -fsSL https://deb.nodesource.com/setup_14.x | bash - \
    && apt-get install -y \
        nodejs \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /src
COPY ["AZLinuxTest/AZLinuxTest.csproj", "AZLinuxTest/"]
RUN dotnet restore "AZLinuxTest/AZLinuxTest.csproj"
COPY . .
WORKDIR "/src/AZLinuxTest"
RUN dotnet build "AZLinuxTest.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "AZLinuxTest.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "AZLinuxTest.dll"]
