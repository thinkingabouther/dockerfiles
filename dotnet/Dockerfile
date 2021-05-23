FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS build
WORKDIR /src
COPY *.sln .
COPY project_name/*.csproj project_name/
COPY project_name.Tests/*.csproj project_name.Tests/

RUN dotnet restore
COPY . .

# testing
FROM build AS testing
WORKDIR /src/project_name
RUN dotnet build
WORKDIR /src/project_name.Tests
RUN dotnet test

# publish

FROM build AS publish
WORKDIR /src/project_name
RUN dotnet publish -c Release -o /src/publish

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1 AS runtime
WORKDIR /app
COPY --from=publish /src/publish .
# heroku uses the following

CMD ASPNETCORE_URLS=http://*:$PORT dotnet project_name.dll
