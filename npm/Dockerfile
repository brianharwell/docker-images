# http://theshravan.net/blog/how-to-use-node-and-npm-without-installation-or-admin-rights

ARG winver

FROM mcr.microsoft.com/powershell:nanoserver-${winver} as build

ARG node_version=10.16.0
ARG npm_version=6.9.0

SHELL ["pwsh.exe", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

WORKDIR /nodejs

RUN Invoke-WebRequest -Uri $('https://nodejs.org/dist/v{0}/win-x64/node.exe' -f $env:node_version) -OutFile node.exe -UseBasicParsing
RUN Invoke-WebRequest -Uri $('https://github.com/npm/cli/archive/v{0}.zip' -f $env:npm_version) -OutFile npm.zip -UseBasicParsing
RUN Expand-Archive -Path npm.zip -DestinationPath node_modules

RUN Rename-Item -Path $('C:/nodejs/node_modules/cli-{0}' -f $env:npm_version) -NewName npm

RUN Copy-Item -Path C:/nodejs/node_modules/npm/bin/npm.cmd -Destination C:/nodejs

### Runtime stage

FROM mcr.microsoft.com/windows/nanoserver:${winver} as runtime

LABEL Description="NPM on Windows"

COPY --from=build /nodejs /nodejs

RUN setx PATH "%PATH%;C:\nodejs;"