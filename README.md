# whatif_install

Launches an [Online WhatIf](https://github.com/AURIN/online-whatif) server.

## Terms

    In the doco below:  
    * ENV = dev | test | prod
    * ORG = asu | cityfutures

# Installing WhatIf

Clone this repo to an Ansible control server.

## Set variables
Check and edit variable values in the inventory file `ENV_ORG` and corresponding group vars `group_vars/ORG` and `group_vars/ENV_ORG`.

Create a new admin password and encrypt it before saving to `group_vars/ENV_ORG`. The password may contain only lower case letters and digits, judging from the example passwords in the `/etc/aurin/envision-combined.properties` file:

`ansible-vault encrypt_string 'PASSWORD'` where PASSWORD is the new password.

## Key pair
Use the AWS Console to create a key pair called 'WhatIf' in your AWS EC2 VPN.

Save the key file and copy it to your Ansible control server as `~/.ssh/WhatIf_ORG_ENV.pem`.

Make it read-only with `chmod 0600 ~/.ssh/WhatIf_ORG_ENV.pem`.

## Security certificate
Generate a security certificate or request it from your network service providers.

When available, copy the certificate and private key files to the Ansible Control server, into the directory roles/install_certificate/files.

Install whatif:

`ansible_playbook -i ENV_ORG playbook.yml --ask-vault-pass`

This will run a collection of playbooks:

* stack.yml: Create server
* prep.yml: Prepare server
* whatif.yml: Install whatif
* cert.yml: Install security certificate
* check.yml: Check site is up before exiting

The install process takes about 45 minutes. Most of this is running `mvn clean package ...` within the installation script `install.sh` called from the task:

`TASK [install_whatif : Install whatif]`

You may SSH to the server to check progress on the install.  To confirm it is still running:

`ps -ef | grep 'clean package'`

You should see output like:

```
root     31761  7899  1 17:08 pts/1    00:00:06 /usr/lib/jvm/java-7-openjdk-amd64/jre/bin/java -classpath /usr/share/maven/boot/plexus-classworlds-2.x.jar -Dclassworlds.conf=/usr/share/maven/bin/m2.conf -Dmaven.home=/usr/share/maven org.codehaus.plexus.classworlds.launcher.Launcher clean package -Ddeployment=development -Dsystem=ali-dev -Daurin.dir=/etc/aurin
```

Once completed you may login to the server.

# Troubleshooting

Use `view /var/log/tomcat7/catalina.out` to check the Tomcat logs for errors.
