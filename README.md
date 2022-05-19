# Action: Deploy Silverstripe

Deploys a Github repository to a host.

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

## Options

... action Options
