#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.


FROM mcr.microsoft.com/dotnet/sdk:5.0 AS base
WORKDIR /src
COPY *.sln .
# copy and restore all projects
COPY DockerDemo/*.csproj DockerDemo/
RUN dotnet restore DockerDemo/*.csproj
COPY DockerDemo.Test/*.csproj DockerDemo.Test/
RUN dotnet restore DockerDemo.Test/*.csproj
# Copy everything else
COPY . .
#Testing
FROM base AS testing
WORKDIR /src/DockerDemo
RUN dotnet build
#WORKDIR /src/DockerDemo.Test
WORKDIR /src/DockerDemo.Test
RUN dotnet test

#Publishing
FROM base AS publish
WORKDIR /src/DockerDemo
RUN dotnet publish -c Release -o /src/publish

#Get the runtime into a folder called app
FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS runtime
WORKDIR /app
COPY --from=publish /src/publish .
#ENTRYPOINT ["dotnet", "DockerDemo.dll"]
CMD ASPNETCORE_URLS=http://*:$PORT dotnet DockerDemo.dll
    
    
 