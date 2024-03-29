# M2 Github actions workflow

## Installation

Clone this repo `git@github.com:Skywire/m2-actions-workflow.git`

Copy the workflows you want to use into your project's `.github/worksflows/` directory


## Available Workflows

### Standard

These should be used on all projects

* commit_lint - lint commit messages against conventional changelog standard
* php - Run PHP test suites
* static - Run static analysis (PHPCS)

### Optional

These are optional or experimental workflows and should be added only as required

* uat - Deploy to UAT via capistrano and run cypress test suite
* production-deploy - Deploy to production via capistrano

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
- Update github configuration as required to not allow merging without this passing

### Deployment

Configure the following secrets

* DESTINATION_HOSTNAME = The hostname or IP to deploy to
* DEP_HOST_ALIAS = The host alias used in deployer
* DESTINATION_HOST_PRIVATE_KEY = The SSH private key to connect to the host, ensure the public key has been added to ~/.ssh/authorized_keys
* KNOWN_HOSTS = The signature of the destination host taken from `ssh-keyscan -H [hostname]`
* SSH_CONFIG_FILE = The contents of the ~/.ssh/config file (if any)
* OPENVPN_CERTIFICATE base64 encoded p12 certificate file

Example SSH_CONFIG_FILE
```
Host mage.hostname
    HostName acc.magestack.com
    User www-data
```

### CI Specific Composer
This package provides a CI specific composer.json and composer.lock, composer-ci.json and composer-ci.lock should be copied to your project root.
If you want to use the same composer for CI and your project uncomment `.github/workflows/production-deploy.yml:46` and comment `.github/workflows/production-deploy.yml:47`

### OpenVPN

If your destination server requires OpenVPN you will need to place an `openvpn.conf` configuration file and any certificates into `.github/workflows/config/openvpn`

To base64 encode the certificate use `base64 <certificate_name>.p12`

e.g
`base64 .github/workflows/config/openvpn/*.p12`

Then copy this value into your OPENVPN_CERTIFICATE secret and delte all p12 files. Do not commit these to github

Edit the openvpn.conf file and replace the `pkcs12` value with `.github/workflows/config/openvpn/openvpn.p12`

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

### Testproject.io

To run testproject tests add the testpoject job to your production-deploy.yaml

```
on:
    ...
jobs
  production_deploy:
    ...
  testproject:
    needs: [deploy]
    uses: ./.github/workflows/testproject.yaml
    secrets: inherit
```

Add testproject secrets

* TP_API_KEY = Your testproject.io API key
* TP_JOB_ID = The job you want to run (you can get this ID from the copy ID link in testproject itself)
