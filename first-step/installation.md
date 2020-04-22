---
description: Build step
---

# Install Friday

## Prerequisites

* [Rust](https://www.rust-lang.org/tools/install)
* [Go](https://golang.org/doc/install) &gt;= 1.13
* [protoc](http://google.github.io/proto-lens/installing-protoc.html) &gt;= 3.6.1
* [rustup](https://rustup.rs/)
* [node](https://nodejs.org/en/download/)
* [git](https://www.git-scm.com/downloads)
* [gcc](https://gcc.gnu.org/git.html) &gt;= 8.3
* make and cmake

{% hint style="warning" %}
You should set PATH environment and other mandatory settings for installed prerequisite packages.
{% endhint %}

## Build

#### Clone repository

```bash
git clone https://github.com/hdac-io/friday.git
```

#### Install

```bash
cd friday

# install required library package first
npm install -g assemblyscript@0.9.1

# build and install Friday
make install
```

If installation succeeded, you can check Friday binaries.

```bash
nodef version
clif version
```

#### Test

```bash
make test
```

