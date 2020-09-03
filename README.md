# M2 Github actions workflow

## Installation

`composer require skywire/m2-actions-workflow`

Commit new files to git.

Files can be customised for your project, composer should not overwrite them when you run an install or update

## Customisation

- Update `.github/workflows/php.yml` to the version of PHP required

- Update `.github/workflows/static.yml` and replace `app/code/[ClientNamespace]` with your client namespace, e.g app/code/FL

- Update `.github/workflows/config/integration/phpunit.xml` and `.github/workflows/config/unit/phpunit.xml` with directories to test:
~~~
<directory suffix="Test.php">../../../app/code/Skywire/*/Test/Integration</directory>
~~~
- Update `.github/workflows/config/integration/phpunit.xml` and whitelist code dir:
~~~
<directory suffix=".php">../../../app/code/Skywire</directory>
~~~
- Remove .travis.yml if exists
- Update github configuration as required to not allow mergeing without this passing

### Capistrano

Configure the following secrets

* DESTINATION_HOSTNAME = The hostname or IP to deploy to
* CAPISTRANO_HOST_ALIAS = The host alias used in capistrano
* DESTINATION_HOST_PRIVATE_KEY = The SSH private key to connect to the host
* KNOWN_HOSTS = The signature of the destination host taken from `ssh-keyscan -H [hostname]`

### OpenVPN

If your destination server requires OpenVPN you can will need to place an `openvpn.conf` configuration file and any certificates into `.github/workflows/config/openvpn`

### Hosts

To add a hosts file entry use 
```yaml
jobs:
  job_name:
    steps:
      - name: set /etc/hosts
        run: |
          echo 'localhost docker.example.com' | sudo tee -a /etc/hosts
```