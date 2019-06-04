ARG redis_version=3.2.100

FROM mcr.microsoft.com/powershell:nanoserver-1809 as build

SHELL ["pwsh.exe", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

RUN Invoke-WebRequest -Uri $("https://github.com/microsoftarchive/redis/releases/download/win-{0}/Redis-x64-{0}.zip" -f ${redis_version}) -OutFile redis.zip -UseBasicParsing; ` \
    Expand-Archive -Path redis.zip -DestinationPath redis;


FROM mcr.microsoft.com/windows/nanoserver:1809 as runtime

LABEL Description="Redis on Windows"

WORKDIR /redis

COPY --from=build /redis .

EXPOSE 6379

CMD [ "redis-server.exe", "--protected-mode no" ]
