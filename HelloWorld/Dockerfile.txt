# Use the official .NET SDK as a build image
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build

# Set the working directory inside the container
WORKDIR /src

# Copy the .NET project file and restore dependencies
COPY HelloWorld.csproj .
RUN dotnet restore

# Copy the entire project and publish the application
COPY . .
RUN dotnet publish -c Release -o /app

# Use the official .NET runtime as a base image
FROM mcr.microsoft.com/dotnet/runtime:6.0 AS base

# Set the working directory inside the container
WORKDIR /app

# Copy the published application from the build image
COPY --from=build /app .

# Set the entry point for the application
ENTRYPOINT ["dotnet", "HelloWorld.dll"]
