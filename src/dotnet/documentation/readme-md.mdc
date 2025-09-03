---
description: A rules for using Cursor to write and update technical documentation files in the .NET solution
globs: README.md
alwaysApply: false
---

# Cursor Rules

This document outlines the rules on how to write or update technical documentation for functionalities and features in this repository.

When developing or updating existing functionality, good technical documentation is invaluable to the engineers that will use these new or updated code, ensuring alignment and reducing development iterations.
 
When asked to generate technical documentation for a functionality or a feature, Cursor should search if there is existing documentation or if it has to create a new one. 



## README Content Best Practices

A good README.md should include:

1. Package name and description
2. Installation instructions
3. Basic usage examples
4. API documentation or link to it
5. License information
6. Contributing guidelines

### Example README.md Structure

```markdown
# MyAwesomePackage

A lightweight, high-performance library for doing awesome things.

## Installation

```shell
dotnet add package MyAwesomePackage
```

## Quick Start

```csharp
using MyAwesomeNamespace;

var awesome = new AwesomeClass();
var result = awesome.DoSomethingAwesome();
```

## Features

- Feature 1: Description
- Feature 2: Description
- Feature 3: Description

## Documentation

For full documentation, visit [our docs site](mdc:https:/docs.myawesomepackage.com).

## License

MIT License
```

### Image Domain Restrictions

When including images in your README.md, ensure they come from trusted domains. NuGet.org only renders images from approved domains.

#### Approved Image Domains

- api.codacy.com
- app.codacy.com
- api.codeclimate.com
- api.dependabot.com
- api.travis-ci.com
- api.reuse.software
- app.fossa.com
- app.fossa.io
- avatars.githubusercontent.com
- badge.fury.io
- badgen.net
- badges.gitter.im
- buildstats.info
- caniuse.bitsofco.de
- camo.githubusercontent.com
- cdn.jsdelivr.net
- cdn.syncfusion.com
- ci.appveyor.com
- circleci.com
- codecov.io
- codefactor.io
- coveralls.io
- dev.azure.com
- flat.badgen.net
- github.com/.../workflows/.../badge.svg
- gitlab.com
- img.shields.io
- i.imgur.com
- isitmaintained.com
- opencollective.com
- raw.github.com
- raw.githubusercontent.com
- snyk.io
- sonarcloud.io
- travis-ci.com
- travis-ci.org
- wakatime.com
- user-images.githubusercontent.com

#### ✅ DO: Use approved domains for images

```markdown
![Build Status](mdc:https:/img.shields.io/github/workflow/status/myorg/myrepo/CI)
![Coverage](mdc:https:/codecov.io/gh/myorg/myrepo/branch/main/graph/badge.svg)
```

#### ❌ DON'T: Use unapproved domains for images

```markdown
![Logo](mdc:https:/my-unapproved-domain.com/logo.png)
```

# End of Cursor Rules File