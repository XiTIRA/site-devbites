+++
title = "First Project"
description = ""
date = 2021-05-01T08:00:00+00:00
updated = 2021-05-01T08:00:00+00:00
draft = false
weight = 10
sort_by = "weight"
template = "docs/page.html"

[extra]
lead = ''
toc = true
top = false
+++
## Getting Started
Getting started is pretty straight forward - regardless of the platform or tool chain that you've decided
on. The most difficult decision will be deciding on a project structure.

Supporting a single platform is simple, you would create a single platform specific project.

## Simple Start

The below will create an empty desktop project supporting OpenGL.

{{< tabs "installdeps" >}}
{{< tab "VS Code / CLI" >}}
```bash
dotnet new mgdekstopgl -o AwesomeGame
cd AwesomeGame
code .
```
{{< /tab >}}
{{< tab "Rider" >}}
![Rider New Project](ridernewproj.png)
{{< /tab >}}
{{< tab "Visual Studio" >}}
![Visual Studio New Project](visualstudionew.png)
{{< /tab >}}
{{< /tabs >}}

## Available Templates

| Cli Name    | Template                           |
|-------------|------------------------------------|
| mgdesktopgl | Cross Platform Desktop Application |
| mgdesktopdx | Windows Desktop Application        |
| mgshared    | MonoGame Shared Library            |
| mglib       | MonoGame Game Library              |
| mgandroid   | MonoGame Android Application       |
| mgpipeline  | Content Pipeline Extension         |
