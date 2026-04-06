## Packer (AMI Build)

Build a custom AWS AMI using Packer:

```bash
cd packer
packer init .
packer validate .
packer build .
