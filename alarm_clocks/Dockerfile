# https://developers.home-assistant.io/docs/add-ons/configuration#add-on-dockerfile
ARG BUILD_FROM

FROM mcr.microsoft.com/dotnet/sdk:8.0.204 as build

WORKDIR /build

COPY . .

RUN dotnet publish src/AlarmClocks --configuration Release --output artifacts --self-contained

FROM $BUILD_FROM as runtime

# Execute during the build of the image
ARG TEMPIO_VERSION BUILD_ARCH
RUN \
    curl -sSLf -o /usr/bin/tempio \
    "https://github.com/home-assistant/tempio/releases/download/${TEMPIO_VERSION}/tempio_${BUILD_ARCH}"

# Copy root filesystem
COPY rootfs /
COPY --from=build /build/artifacts /usr/bin
