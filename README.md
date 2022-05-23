# Example multi-arch Python devcontainer

This sample shows how to install Python and a number of other tools as multi-architecture within a devctonainer so it will run on both x86/64 and ARM architectures (i.e. Mac M1 chipsets).

Where multi-arch tools aren't available I query the running architecture and then install the corresponding version.

Tools included:
- Python3
- Terraform
- Docker
- Azure CLI
