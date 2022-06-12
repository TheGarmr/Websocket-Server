FROM mcr.microsoft.com/dotnet/sdk:6.0-alpine AS publish

WORKDIR /etc/websocket-support

COPY ["WebSocket.Server/WebSocket.Server.csproj", "WebSocket.Server/"]

RUN dotnet restore "WebSocket.Server/WebSocket.Server.csproj" --runtime alpine-x64
COPY . .

RUN dotnet publish "WebSocket.Server/WebSocket.Server.csproj" \
    --configuration Release \
    --output /etc/websocket-support/publish \
    --no-restore \
    --nologo \
    --runtime alpine-x64 \
    --self-contained true \
    --verbosity quiet \
    /p:PublishTrimmed=true \
    /p:PublishSingleFile=true

FROM mcr.microsoft.com/dotnet/runtime-deps:6.0-alpine AS final

RUN apk add tzdata && cp /usr/share/zoneinfo/Europe/Kiev /etc/localtime && apk upgrade musl

WORKDIR /etc/websocket-support
EXPOSE 80
COPY --from=publish /etc/websocket-support/publish .
ENTRYPOINT ["./WebSocket.Server"]