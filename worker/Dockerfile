FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
 
WORKDIR /source
 
COPY Worker.csproj .
RUN dotnet nuget locals all --clear && dotnet restore --no-cache --verbosity minimal
 
COPY . .
RUN dotnet publish -c Release -o /app
 
FROM mcr.microsoft.com/dotnet/runtime:7.0
 
WORKDIR /app
 
COPY --from=build /app .
 
ENTRYPOINT ["dotnet", "Worker.dll"]