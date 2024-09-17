This repo is an issue reprod for Datadog Tracer 3.3.0.

When building for .NET 6 with the latest tracer version, various shell commands will fail, such as `cat`, `clear`, etc.

Steps to Reproduce:
1. `docker build --file .\Dockerfile --tag ddtracer:test .`
2. `docker run --rm -it --entrypoint /bin/bash ddtracer:test`
3. Try `cat` or `clear`, and it should fail with the error below.

Error
>cat: symbol lookup error: /opt/datadog/continuousprofiler/Datadog.Linux.ApiWrapper.x64.so: undefined symbol: dlsym

Some noteworthy things:

If you hardcode the version to 3.2.0, it works.

```
RUN mkdir -p /opt/datadog \
    && mkdir -p /var/log/datadog \
    && TRACER_VERSION="3.2.0" \
    && curl -LO https://github.com/DataDog/dd-trace-dotnet/releases/download/v${TRACER_VERSION}/datadog-dotnet-apm_${TRACER_VERSION}_amd64.deb \
    && dpkg -i ./datadog-dotnet-apm_${TRACER_VERSION}_amd64.deb \
    && rm ./datadog-dotnet-apm_${TRACER_VERSION}_amd64.deb \
    && /opt/datadog/createLogPath.sh
```

It also works if you change the dotnet runtime to 8.0.
