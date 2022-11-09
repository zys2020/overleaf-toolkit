# Overleaf Toolkit

This repository customizes a convenient deployment for a local instance of overleaf, which integrates all docker-compose files that comes from the standard tools for running a local
instance of [Overleaf](https://overleaf.com) into a single docker-compose file. This repository will help you to set up and administer Overleaf Community Edition (free to use, and community supported).

- refer to: 
    - https://github.com/overleaf/overleaf/wiki/Quick-Start-Guide
    - https://yeasy.gitbook.io/docker_practice/compose


## Deployment

```sh
$ git clone git@github.com:zys2020/overleaf-toolkit.git ./overleaf-toolkit
$ cd overleaf-toolkit/lib
$ docker-compose -p overleaf -f docker-compose.overleaf.yml up -d  # backward end 
```
It will take a long time to install packages used in daily academic writing.
# Previous Guidelines

## Getting Started

Clone this repository locally:

``` sh
$ git clone git@github.com:zys2020/overleaf-toolkit.git
```

Then follow the [Quick Start Guide](./doc/quick-start-guide.md).


## Documentation

See [Documentation Index](./doc/README.md)


## Contributing

See the [CONTRIBUTING](https://github.com/overleaf/overleaf/blob/main/CONTRIBUTING.md) file.


## Getting Help

Users of the free Community Edition should [open an issue on github](https://github.com/overleaf/toolkit/issues). 

Users of Server Pro should contact `support@overleaf.com` for assistance.

In both cases, it is a good idea to include the output of the `bin/doctor` script in your message.

