# builder
FROM mcr.microsoft.com/dotnet/sdk:5.0-alpine AS build
ARG BUILD_VERSION=1.0.0

WORKDIR /src
COPY src/Versioned.Api/Versioned.Api.csproj ./Versioned.Api/
RUN dotnet restore /src/Versioned.Api/Versioned.Api.csproj

COPY src/Versioned.Api/ ./Versioned.Api/
RUN echo "Publishing Core.API - build version: ${BUILD_VERSION}" && \
    dotnet publish /src/Versioned.Api/Versioned.Api.csproj -c Release /p:Version=$BUILD_VERSION -o /out

FROM build AS publish
RUN dotnet publish "/Core.API/Core.API.csproj" -c Release -o /app/publish

# app
FROM mcr.microsoft.com/dotnet/aspnet:5.0-alpine
ARG BUILD_TAG=local
ARG COMMIT_SHA=local

EXPOSE 80
ENTRYPOINT ["dotnet", "/app/Versioned.API.dll"]

COPY --from=build /out/ /app

LABEL build_tag=${BUILD_TAG}
LABEL commit_sha=${COMMIT_SHA}