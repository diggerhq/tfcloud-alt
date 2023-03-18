UPDATE
======
Please visit more mature repository here: https://github.com/diggerhq/digger

Introduction
======

Do we really need terraform cloud? TerraformCloud, Spacelift and other cloud runners present dedicated CI/CD to run terraform. 

Why are people using Terraform Cloud? Why not run Terraform in Github Actions? For my usecase as a small startup it seems to me that relying on github actions should be enough. I might be missing something but it seems that I need from a terraform runner is the following flow:

- Runs terraform plan on every PR
- Runs terraform apply on merge to master/main branch
- Handle of concurrency by queining multiple applies together

More advanced flows such as multiple environments and comment-to-apply can also be acheived as an extension of above. So why do we need a dedicated service
to run our terraform ? What other features or caveats have I not thought of yet?

How to run
=====

Bring your own terraform
-----

If you have terraform already on github and you would like to use this then you can do the following:

- Create directory `.github/workflows`
- Create the following workflow for plans `.github/workflows/tfplan.yml`

```
name: Create terraform plan

on: [pull_request]

permissions:
  contents: read
  pull-requests: write

jobs:
  plan:
    runs-on: ubuntu-latest
    name: Create a plan for an example terraform configuration
    env:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
      AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: terraform plan
        uses: dflook/terraform-plan@v1
        with:
          path: terraform_examples
          variables: |
            xxxxx = "${{ secrets.xxxxx }}"
```

- Create the following workflow for applies `.github/workflows/tfapply.yml`

```
name: Apply terraform plan

on:
  push:
    branches:
      - main

permissions:
  contents: read
  pull-requests: write

jobs:
  apply:
    runs-on: ubuntu-latest
    name: Apply terraform plan
    env:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
      AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}

    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: terraform apply
        uses: dflook/terraform-apply@v1
        with:
          path: terraform_examples
          variables: |
            xxxxx = "${{ secrets.xxxxx }}"

```


Use sample repository
------

You can also play with this sample repository which contains sample code. To do this you need to fork the repository and set the following secrets:

```
AWS_ACCESS_KEY_ID=xxxxx
AWS_SECRET_ACCESS_KEY=yyyyyy
DB_PASSWORD=pwdpwdpwdpwd
```

And you can create a PR with some terraform change to trigger the action.

Thanks
=====

I used this terraform action for the demo: https://github.com/dflook/terraform-github-actions

