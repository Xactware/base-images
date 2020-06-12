# Xactware Base Docker Images

Defines the base images used at Xactware

## xactware/dotnet

Base images for .NET Core Runtimes

### Supported tags and respective Dockerfile links

* [2.2-runtime](https://github.com/Xactware/base-images/blob/master/dotnet/core/runtime/2.2/Dockerfile)
* [2.2-aspnetcore-runtime](https://github.com/Xactware/base-images/blob/master/dotnet/core/aspnet/2.2/Dockerfile)
* [3.1-runtime](https://github.com/Xactware/base-images/blob/master/dotnet/core/runtime/3.1/Dockerfile)
* [3.1-aspnetcore-runtime](https://github.com/Xactware/base-images/blob/master/dotnet/core/aspnet/3.1/Dockerfile)

### How To Use these Images

```Dockerfile
FROM xactware/dotnet:2.2-aspnetcore-runtime
ARG source
WORKDIR /app

COPY ${source:-bin/Release/netcoreapp2.2/publish} .

ENTRYPOINT [ "dotnet", "Api.dll" ]
```

```bash
docker build -t myapp .
```

### Dynatrace Support

To enable Dynatrace support you must provide the `DT_ENVIRONMENT_ID` and `DT_PAAS_TOKEN` build arguments.

If you are an employee of Xactware these values should be provided by your build pipeline, and will not be included in this repo or baked into the base image for obvious reasons.

If you are a member of the community, and would like to use this feature you will need to have your own account with Dynatrace to enable this feature.

```bash
docker build -t myapp --build-arg DT_ENVIRONMENT_ID={environmentId} --build-arg DT_PAAS_TOKEN={token} .
```

This will include the `oneagent` in your container. However, this does not fully enable Dynatrace until runtime. In order to enable Dynatrace you must start the container with the `LD_PRELOAD` environment variable.

```bash
docker run --rm -e LD_PRELOAD="/opt/dynatrace/oneagent/agent/lib64/liboneagentproc.so" myapp
```

> *NOTE:* This is an area we would like to simplify. It sucks a lot to have to know this string, but it's the only way we've found so far to be able to toggle Dynatrace on and off at runtime, which we need because we can only enable Dynatrace in certain environments.
