This repo is an issue reprod for Datadog Tracer 3.3.0.

When building for .NET 6 with the latest tracer version, various shell commands will fail, such as `cat`, `clear`, etc.

Steps to Reproduce:
1. `docker build --file .\Dockerfile --tag ddtracer:test .`
2. `docker run --rm -it --entrypoint /bin/bash ddtracer:test`
3. Try `cat` or `clear`, and it should fail with the error below.

Error
>cat: symbol lookup error: /opt/datadog/continuousprofiler/Datadog.Linux.ApiWrapper.x64.so: undefined symbol: dlsym
