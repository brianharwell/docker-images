# ARG build=mcr.microsoft.com/powershell:nanoserver-1803
# ARG runtime=mcr.microsoft.com/windows/nanoserver:1803

ARG build=mcr.microsoft.com/powershell:nanoserver-1709
ARG runtime=mcr.microsoft.com/windows/nanoserver:1709

FROM ${build} as build

SHELL ["pwsh.exe", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

RUN Invoke-WebRequest -Uri "https://github.com/microsoftarchive/redis/releases/download/win-3.2.100/Redis-x64-3.2.100.zip" -OutFile redis.zip -UseBasicParsing; ` \
    Expand-Archive -Path redis.zip -DestinationPath redis;


FROM ${runtime} as runtime

LABEL Description="Redis on Windows"

WORKDIR /redis

COPY --from=build /redis .

EXPOSE 6379

CMD [ "redis-server.exe", "--protected-mode no" ]
