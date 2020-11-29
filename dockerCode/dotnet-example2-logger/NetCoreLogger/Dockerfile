# escape=`

### 1: Build/Publish .NET Core Web Application ###
#Base Image from Microsoft for Building and Publishing the application
FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS build

#set up a directory in container
WORKDIR /scr

#copy sourceCode to /src folder on container
COPY NetCoreLogger.Web/ .

#restore nuget packages -- #RUN: Used to execute a command during the build process of the docker image
RUN dotnet restore "NetCoreLogger.Web.csproj"

#build the application
RUN dotnet build "NetCoreLogger.Web.csproj" -c Release -o /src/build

#publish the application as a publish-stage
FROM build AS publish
RUN dotnet publish "NetCoreLogger.Web.csproj" -c Release -o /src/publish

### 2: RunTime ###
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1 AS base

#set up a directory in container
WORKDIR /app

#copy the published output of .NET Core code
COPY --from=publish /src/publish . 

WORKDIR /app

CMD ["./NetCoreLogger.Web"]




###########   Commands/Links##########################
#docker image build -t netcorelogger .
#docker run --rm --name netcorelogger-docker -p 5000:5000 netcorelogger
#https://www.plcnext-community.net/en/hn-makers-blog/456-getting-started-with-c-and-visual-studio-code-on-plcnext.html (system.globalization error fix)
