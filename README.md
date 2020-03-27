# M2 Github actions workflow

## Installation

`composer require skywire/m2-actions-workflow`

Commit new files to git.

Files can be customised for your project, composer should not overwrite them when you run an install or update

## Customisation

- Update `.github/workflows/php.yml` to the version of PHP required
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

## Usage

Contains unit and integration test runners
