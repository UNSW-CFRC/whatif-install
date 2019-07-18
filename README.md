# whatif_install

Generates a WhatIf server.

## Terms

    In the doco below:  
    * ENV = dev | test | prod
    * ORG = asu | cityfutures

# Installing WhatIf

Clone this repo to an Ansible control server.

Check and edit variable values in the inventory file ENV_ORG and corresponding files in the group_vars folder.

Install whatif:

`ansible_playbook -i ENV_ORG playbook.yml --ask-vault-pass`
