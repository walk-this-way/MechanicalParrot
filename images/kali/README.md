# Note the key in packer.pem is a dummy and doesn't correspond to any authentication. You need to replace it.


# Install Dependencies
 - chocolatey (windows only)
 - packer
 - ansible - apparently this doesn't work on windows - https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html
 - vmware workstation (if building for vmware)
 - need `kali-linux-2020.4-live-amd64.iso` (if building for vmware) downloaded and saved in this directory. This was really slow and I had to torrent it. It may be worth downloading other large files and putting them in the `http` directory. Then they can be downloaded from the VM host for much faster builds. Normally I'd just have the script download this, but caching it improves speed by a lot.
 - need gitlab-runner installed locally to run the CI tests locally - https://docs.gitlab.com/runner/install/linux-manually.html

# Configure
 - You probably want to change the ssh public key in the vars.

# Run prereqs
For AWS need access keys, and an SSH key named packer, with a filename of packer.pem (the current packer.pem is a dummy that needs to be replaced)

# Build vmware
 - `packer build --only=vmware-iso kali.json`

# Build AWS
 - `packer build -var `aws_access_key=XXX` -var `aws_secret_key=YYY` --only=amazon-ebs kali.json`


Accounts:
kali (is sudoer) (can only ssh with ssh key)




## TODO - Windows example
https://yetiops.net/posts/packer-ansible-windows-aws/

## other kali ansible installation resources (not sure what we want right now):
https://github.com/iesplin/ansible-playbook-kali
https://github.com/cisagov/ansible-role-kali


## Idea for packer integration with gitlab
https://virtualhobbit.com/2020/05/05/using-gitlab-ci-cd-pipelines-to-automate-your-hashicorp-packer-builds/

## Useful for getting CICD to work. IMPORTANT
Always test locally before pushing it to gitlab for cost saving, and for fast feedback
https://www.lambdatest.com/blog/use-gitlab-ci-to-run-test-locally/

`gitlab-runner exec docker packer-kali`

## This is why we can't have nice things:
https://gitlab.com/gitlab-org/gitlab-runner/-/issues/2587

## Future ideas for storing secrets (ssh keys, etc)
https://dx.appirio.com/ci-cd-gitlab/gitlab-env-vars/

## Might want to have the admin install a shared runner at some point (so we don't run out of free CICD, depending on how much we use????)
https://docs.gitlab.com/ee/ci/runners/README.html
