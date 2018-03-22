# Develop .NET Core Applications in a Container

You can use containers to establish a .NET Core development environment with only Docker and optionally a code editor installed on your machine. The environment can be made to match your local machine, production or both. If you support multiple operating systems, then this approach might become a key part of your development process.

A common use case of Docker is to [containerize an application](README.md). You can define the environment necessary to run the application and even build the application itself within a Dockerfile. This document describes a much more iterative and dynamic use of Docker, defining the container environment primarily via the commandline. .NET Core includes a command called `dotnet watch` that can rerun your application or your tests on each code change. This document describes how to use the Docker CLI and `dotnet watch` to develop applications in a container.

See [Develop ASP.NET Core Applications in a Container](../aspnetapp/aspnet-docker-dev-in-container.md) for ASP.NET Core-specific instructions.

## Getting the sample

The easiest way to get the sample is by cloning the samples repository with [git](https://git-scm.com/downloads), using the following instructions:

```console
git clone https://github.com/dotnet/dotnet-docker/
```

You can also [download the repository as a zip](https://github.com/dotnet/dotnet-docker/archive/master.zip).

## Requirements

The instructions below use .NET Core 2.1 Preview 2 images. It is possible to make this scenario work with .NET Core 2.0 but requires many extra steps and a bit of magic. You do not need to switch to .NET Core 2.1 on your local machine to try out these instructions. They will work fine with .NET Core 2.0 projects.

It is recommended that you add a [Directory.Build.props](Directory.Build.props) file to your project to use different `obj` and `bin` folders for local and container use, to avoid conflicts between them. You should delete your existing obj and bin folders before making this change. You can also use `dotnet clean` for this purpose.

This approach relies on [volume mounting](https://docs.docker.com/engine/admin/volumes/volumes/) (that's the `-v` argument in the following commands) to mount source into the container (without using a Dockerfile). You may need to [Enable shared drives (Windows)](https://docs.docker.com/docker-for-windows/#shared-drives) or [file sharing (macOS)](https://docs.docker.com/docker-for-mac/#file-sharing) first.

## Run your application in a container while you Develop

You can rerun your application in a container with every local code change. This scenario works for both console applications and websites. The syntax differs a bit for Windows and Linux containers.

The instructions assume that you are in the root of the repository. You can use the following commands, given your environment:

**Windows** using **Linux containers**

```console
docker run --rm -it -v c:\git\dotnet-docker\samples\dotnetapp:/app/ -w /app/dotnetapp microsoft/dotnet-nightly:2.1-sdk dotnet watch run
```

**Linux or macOS** using **Linux containers**

```console
docker run --rm -it -v ~/git/dotnet-docker/samples/dotnetapp:/app/ -w /app/dotnetapp microsoft/dotnet-nightly:2.1-sdk dotnet watch run
```

**Windows** using **Windows containers**

```console
docker run --rm -it -v c:\git\dotnet-docker\samples\dotnetapp:c:\app\ -w \app\dotnetapp microsoft/dotnet-nightly:2.1-sdk dotnet watch run
```

## Test your application in a container while you develop

You can retest your application in a container with every local code change. This works for both console applications and websites. The syntax differs a bit for Windows and Linux containers.

The instructions assume that you are in the root of the repository. You can use the following commands, given your environment:

**Windows** using **Linux containers**

```console
docker run --rm -it -v c:\git\dotnet-docker\samples\dotnetapp:/app/ -w /app/tests microsoft/dotnet-nightly:2.1-sdk dotnet watch test
```

**Linux or macOS** using **Linux containers**

```console
docker run --rm -it -v ~/git/dotnet-docker/samples/dotnetapp:/app/ -w /app/tests microsoft/dotnet-nightly:2.1-sdk dotnet watch test
```

**Windows** using **Windows containers**

```console
docker run --rm -it -v c:\git\dotnet-docker\samples\dotnetapp:c:\app\ -w \app\tests microsoft/dotnet-nightly:2.1-sdk dotnet watch test
```

The commands above log test results to the console. You can additionally log results as a TRX file by appending `--logger:trx` to the previous test commands, specifically `dotnet watch test --logger:trx`. TRX logging is also demonstrated in [Running .NET Core Unit Tests with Docker](dotnet-docker-unit-testing.md).

## More Samples

* [.NET Core Docker Samples](../README.md)
* [.NET Framework Docker Samples](https://github.com/microsoft/dotnet-framework-docker-samples/)