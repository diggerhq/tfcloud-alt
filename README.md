Introduction
======

Do we really need terraform cloud? TerraformCloud, Spacelift and other cloud runners offer another tool to run terraform. Why are people using Terraform Cloud? 
Why not run Terraform in Github Actions? I've built a simple action that seems to be enough. I might be missing something but it seems that all you need
from a terraform runner is the following for a basic flow:

- Runs terraform plan on every PR
- Runs terraform apply on merge to master/main branch
- Handle of concurrency by queining multiple applies together



How to run
=====

Bring your own terraform
-----


Use sample repository
------


Roadmap
======


