# escape=`
FROM mcr.microsoft.com/windows/nanoserver:sac2016

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

# Install ASP.NET Core Runtime
ENV ASPNETCORE_VERSION 2.1.8

RUN Invoke-WebRequest -OutFile aspnetcore.zip https://dotnetcli.blob.core.windows.net/dotnet/aspnetcore/Runtime/$Env:ASPNETCORE_VERSION/aspnetcore-runtime-$Env:ASPNETCORE_VERSION-win-x64.zip; `
    $aspnetcore_sha512 = 'b9911481b839c0fd119bcc82c60af32599125be1f4cfd72001f7bf24a21e8986f45930b9fe0d77e07127d78b9b4046f0dc6b3d11f1b238f867e2f42808be0fd9'; `
    if ((Get-FileHash aspnetcore.zip -Algorithm sha512).Hash -ne $aspnetcore_sha512) { `
        Write-Host 'CHECKSUM VERIFICATION FAILED!'; `
        exit 1; `
    }; `
    `
    Expand-Archive aspnetcore.zip -DestinationPath  $Env:ProgramFiles\dotnet; `
    Remove-Item -Force aspnetcore.zip
RUN setx /M PATH $($Env:PATH + ';' + $Env:ProgramFiles + '\dotnet')

# Configure web servers to bind to port 80 when present
ENV ASPNETCORE_URLS=http://+:80 `
    # Enable detection of running in a container
    DOTNET_RUNNING_IN_CONTAINER=true `
    # Deprecated, use `DOTNET_RUNNING_IN_CONTAINER` instead - https://github.com/dotnet/dotnet-docker/issues/677
    DOTNET_RUNNING_IN_CONTAINERS=true
