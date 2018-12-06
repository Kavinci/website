FROM microsoft/dotnet:2.1-aspnetcore-runtime AS base
RUN apt-get update && \
    apt-get install -y wget && \
    apt-get install -y gnupg2 && \
    wget -qO- https://deb.nodesource.com/setup_11.x | bash - && \
    apt-get install -y build-essential nodejs && \
	npm install -g -y typescript && \
	npm install -g -y @angular/cli
WORKDIR /app
EXPOSE 63462
EXPOSE 44336

FROM microsoft/dotnet:2.1-sdk AS build
WORKDIR /src
COPY ["kentutterback/kentutterback.csproj", "kentutterback/"]
RUN dotnet restore "kentutterback/kentutterback.csproj"
COPY . .
WORKDIR "/src/kentutterback"
RUN dotnet build "kentutterback.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "kentutterback.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "kentutterback.dll"]