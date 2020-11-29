# escape=`

### 1: Build/Publish .NET Core Web Application ###

#Base Image from Microsoft for Building and Publishing the application
FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build

#set up a directory in container
WORKDIR /scr

#copy sourceCode to /src folder on container
COPY Web/ .

#restore nuget packages -- #RUN: Used to execute a command during the build process of the docker image
RUN dotnet restore "Web.csproj"

#build the application
RUN dotnet build "Web.csproj" -c Release -r linux-x64 -o /src/build

#publish the application as a publish-stage
FROM build AS publish
RUN dotnet publish "Web.csproj" -c Release -r linux-x64 --self-contained true -o /src/publish

### 2: Run on Ubuntu ###

FROM ubuntu
RUN apt-get update
RUN apt-get install -y
RUN apt-get install libssl1.0.0


#set up a directory in container
WORKDIR /app

#copy the published output of .NET Core code
COPY --from=publish /src/publish . 

WORKDIR /app

CMD ["./Web"]




###########   Commands/Links##########################
#docker image build -t webexample1 .
#docker run --rm --name webexample1-docker -p 5000:5000 webexample1
#https://www.plcnext-community.net/en/hn-makers-blog/456-getting-started-with-c-and-visual-studio-code-on-plcnext.html (system.globalization error fix)
