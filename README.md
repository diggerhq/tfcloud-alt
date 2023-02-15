Introduction
======

Do we really need terraform cloud? TerraformCloud, Spacelift and other cloud runners offer another tool to run terraform. Why are people using Terraform Cloud? 
Why not run Terraform in Github Actions? I've built a simple action that seems to be enough. I might be missing something but it seems that all you need
from a terraform runner is the following for a basic flow:

- Runs terraform plan on every PR
- Runs terraform apply on merge to master/main branch
- Handle of concurrency by queining multiple applies together

More advanced flows such as multiple environments and comment-to-apply can also be acheived as an extension of above. So why do we need a dedicated service
to run our terraform ? 

How to run
=====

Bring your own terraform
-----

If you have terraform already on github and you would like to use this then you can do the following:

- Create directory `.github/workflows`
- Create the following workflow for plans `.github/workflows/tfplan.yml`

```
```

- Create the following workflow for applies `.github/workflows/tfapply.yml`

```
```


Use sample repository
------

You can also play with this sample repository which contains sample code. To do this you need to fork the repository and set the following secrets:

```
AWS_ACCESS_KEY=xxxxx
AWS_SECRET_ACCESS_KEY=yyyyyy
```

And yo ucan create a PR with some terraform change to trigger the action.


FAQ
=====

- How can I implement atlantis workflow using this?

- How to handle secrets and variables in terraform?

- How do I introduce policy-as-code into my actions?
