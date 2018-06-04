# hmmh copr repository

Collection of rpm spec files for smoothening our Fedora provisioning.

## License

Apache 2.0. Please see text of the license in LICENSE file.

## Installation

* Enable copr repository (f.e. for https://copr.fedorainfracloud.org/coprs/johu/hmmh/).

```bash
$ sudo dnf copr enable johu/hmmh
```

## Development

### Prerequisites

* Install needed packages.

```bash
$ sudo dnf install fedora-packager rpmdevtools mock mock-scm copr-cli
```

* Setup `mock` permissions.

```bash
$ sudo usermod -a -G mock YourUserName
$ newgrp mock
```

* Configure copr-cli in `~/.config/copr`. Credentials for login, username and token are  secret.

```bash
[copr-cli]
login =
username =
token =
copr_url = https://copr.fedorainfracloud.org
```

* Clone the github repo in your projects dir (in this example in `~/src/fedora`)

```bash
$ mkdir -p ~/src/fedora
$ cd ~/src/fedora
$ git clone https://github.com/hmmh/copr.git
```

* Configure `~/.rpmbuild`

```diff
- %_topdir %(echo $HOME)/rpmbuild
+ %_topdir %(echo $HOME)/src/fedora/copr
```

### Workflow

* Create a `spec` file in `~/src/fedora/SPECS`
* Download the sources to `~/src/fedora/SOURCES`
* Build the package (replace the name example with the real name)

```bash
$ cd ~/src/fedora/copr/SPECS
$ rpmbuild -ba example.spec
```
* If the package is ready to be published then do so with `copr-cli (replace the name example with the real name)

```bash
$ cd ~/src/fedora/
$ copr-cli build hmmh SPECS/example.spec
```

* Don't forget to add the spec file to the repo (replace the name example with the real name)

```bash
$ cd ~/src/fedora/copr
$ git add SPECS/example.spec
$ git commit
$ git push
```
