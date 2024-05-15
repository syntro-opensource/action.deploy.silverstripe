# Action: Deploy Silverstripe

Deploys a Github repository to a host using ansible and the
[`ansible.silverstripe`](https://github.com/syntro-opensource/ansible.silverstripe)
role.

## Inputs

| Name                 | Required | Secret | Default                                       | Description                                                                               |
| -------------------- |:--------:|:------:| --------------------------------------------- | ----------------------------------------------------------------------------------------- |
| `host`               |    ✅    |        | -                                             | The hostname or IP to deploy to                                                           |
| `host_user`          |    ✅    |        | -                                             | The username with which to log in                                                         |
| `private_key`        |    ✅    |   🔐   | -                                             | The private key to log in (passwords not supported)                                       |
| `working_dir`        |    ✅    |        | -                                             | The directory in which to deploy. The webroot will be in a subdirectory.                  |
| `base_url`           |    ✅    |        | -                                             | The `SS_BASE_URL` of the page                                                             |
| `admin_username`     |    ✅    |   🔐   | -                                             | The username of the default admin                                                         |
| `admin_password`     |    ✅    |   🔐   | -                                             | The Password of the default admin                                                         |
| `database_class`     |          |        | `MySQLPDODatabase`                            | The name of the database class to use                                                           |
| `database_name`      |    ✅    |        | -                                             | The name of the database to use                                                           |
| `database_username`  |    ✅    |   🔐   | -                                             | The username to access the database                                                       |
| `database_password`  |    ✅    |   🔐   | -                                             | The password to access the database                                                       |
| `current_dir`        |          |        | `html`                                        | The name of the webroot. the webroot will be at `< working_dir >/< current_dir >`         |
| `database_server`    |          |        | `localhost`                                   | The database server                                                                       |
| `environment_type`   |          |        | `live`                                        | One of `dev`, `test` and `live`.                                                          |
| `htaccess_template`  |          |        | `htaccess.public.j2`                          | Used to render a custom htaccess in the webroot. Path is relative to your repository root |
| `project_repository` |          |        | `git@github.com:${{ github.repository }}.git` | Use a custom origin. This can be used to use custom configs for diffrent repositories.    |
| `composer_command`   |          |        | -                                             | Use a custom composer command instead of the installed one |
| `error_log`          |          |        | `../../../logs/ss_log.log`                    | The Location of the error log                                                             |

> This is an ongoing project, in the future we want to add more features of the [`ansible.silverstripe`](https://github.com/syntro-opensource/ansible.silverstripe) role.

## Usage Example

To make use of this action, add the following to your workflows:

```yml
# .github/workflows/deploy.yml
name: 🚀 Deployment
on:
  release:
    types:
      - published
  push:
    branches:
      - master
    paths-ignore:
      - .chglog/**
      - .github/**
      - '!.github/workflows/deploy.yml'
      - public/**
      - '*.md'
jobs:
  production:
    name: 👔 Deploy to Production
    runs-on: ubuntu-latest
    if: ${{ github.event_name == 'release' }}
    steps:
    - name: Checkout code
      uses: actions/checkout@v3
    - name: Deploy to server
      uses: syntro-opensource/action.deploy.silverstripe@master
      with:
        host: example.com
        host_user: demouser
        private_key: ${{ secrets.PRIVATE_KEY }}
        working_dir: path/to/deployment                 # see https://github.com/syntro-opensource/ansible.silverstripe#the-webroot-and-the-files-generated
        base_url: https://example.com
        admin_username: ${{ secrets.SS_ADMIN_USER }}
        admin_password: ${{ secrets.SS_ADMIN_PASS }}
        database_server: localhost
        database_name: silverstripedb
        database_username: ${{ secrets.SS_DB_USER }}
        database_password: ${{ secrets.SS_DB_PASS }}

```
