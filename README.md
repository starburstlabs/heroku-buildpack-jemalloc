# heroku-buildpack-jemalloc

[jemalloc](http://jemalloc.net/) is a general purpose malloc implementation
that works to avoid memory fragmentation in multithreaded applications. This
buildpack makes it easy to install and use jemalloc on Heroku and compatible
platforms.

## Install

```console
heroku buildpacks:add --index 1 https://github.com/gaffneyc/heroku-buildpack-jemalloc.git
git push heroku master
```

## Made possible by Dead Man's Snitch

Continued development and support of the jemalloc buildpack is sponsored by
[Dead Man's Snitch](https://deadmanssnitch.com).

Ever been surprised that a critical recurring job was silently failing to run?
Whether it's backups, cache clearing, sending invoices, or whatever your
application depends on, Dead Man's Snitch makes it easy to
[monitor heroku scheduler](https://deadmanssnitch.com/docs/heroku) tasks or to add
[cron job monitoring](https://deadmanssnitch.com/docs/cron-job-monitoring) to
your other services.

Get started for free today with [Dead Man's Snitch on Heroku](https://elements.heroku.com/addons/deadmanssnitch)

## Usage

### Recommended

Set the JEMALLOC_ENABLED config option to true and jemalloc will be used for
all commands run inside of your dynos.

```console
heroku config:set JEMALLOC_ENABLED=true
```

### Per dyno

To control when jemalloc is configured on a per dyno basis use
`jemalloc.sh <cmd>` and ensure that JEMALLOC_ENABLED is unset.

Example Procfile:
```yaml
web: jemalloc.sh bundle exec puma -C config/puma.rb
```

## Configuration

### JEMALLOC_ENABLED

Set this to true to automatically enable jemalloc.

```console
heroku config:set JEMALLOC_ENABLED=true
```

To disable jemalloc set the option to false. This will cause the application to
restart disabling jemalloc.

```console
heroku config:set JEMALLOC_ENABLED=false
```

### JEMALLOC_VERSION

Set this to select or pin to a specific version of jemalloc. The default is to
use the latest stable version if this is not set. You will receive an error
mentioning tar if the version does not exist.

**Default**: `5.2.1`

**note:** This setting is only used during slug compilation. Changing it will
require a code change to be deployed in order to take affect.

```console
heroku config:set JEMALLOC_VERSION=3.6.0
```

#### Available Versions

| Version                                                          | Released   |
| ---------------------------------------------------------------- | ---------- |
| [3.6.0](https://github.com/jemalloc/jemalloc/releases/tag/3.6.0) | 2015-04-15 |
| [4.0.4](https://github.com/jemalloc/jemalloc/releases/tag/4.0.4) | 2015-10-24 |
| [4.1.1](https://github.com/jemalloc/jemalloc/releases/tag/4.1.1) | 2016-05-03 |
| [4.2.1](https://github.com/jemalloc/jemalloc/releases/tag/4.2.1) | 2016-06-08 |
| [4.3.1](https://github.com/jemalloc/jemalloc/releases/tag/4.3.1) | 2016-11-07 |
| [4.4.0](https://github.com/jemalloc/jemalloc/releases/tag/4.4.0) | 2016-12-04 |
| [4.5.0](https://github.com/jemalloc/jemalloc/releases/tag/4.5.0) | 2017-02-28 |
| [5.0.1](https://github.com/jemalloc/jemalloc/releases/tag/5.0.1) | 2017-07-01 |
| [5.1.0](https://github.com/jemalloc/jemalloc/releases/tag/5.1.0) | 2018-05-08 |
| [5.2.0](https://github.com/jemalloc/jemalloc/releases/tag/5.2.0) | 2019-04-02 |
| [5.2.1](https://github.com/jemalloc/jemalloc/releases/tag/5.2.1) | 2019-08-05 |

The complete and most up to date list of supported versions and stacks is
available on the [releases page.](https://github.com/gaffneyc/heroku-buildpack-jemalloc/releases)

## Building

This uses Docker to build against Heroku
[stack-image](https://github.com/heroku/stack-images)-like images.

```console
make VERSION=5.1.0
```

Artifacts will be dropped in `dist/` based on Heroku stack and jemalloc version.

### Deploying New Versions

- `make VERSION=X.Y.Z`
- `open dist`
- Go to [releases](https://github.com/gaffneyc/heroku-buildpack-jemalloc/releases)
- Edit the release corresponding to each heroku Stack
- Drag and drop the new build to attach

### Creating a New Stack
- Go to [releases](https://github.com/gaffneyc/heroku-buildpack-jemalloc/releases)
- Click "Draft a new release"
- Tag is the name of the Stack (e.g. `heroku-18`)
- Target is `release-master`
- Title is `Builds for the [stack] stack`
