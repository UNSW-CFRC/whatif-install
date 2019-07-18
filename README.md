# whatif_install

Launches an [Online WhatIf](https://github.com/AURIN/online-whatif) server.

## Terms

    In the doco below:  
    * ENV = dev | test | prod
    * ORG = asu | cityfutures

# Installing WhatIf

Clone this repo to an Ansible control server.

Check and edit variable values in the inventory file ENV_ORG and corresponding files in the group_vars folder.

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

If concerned, you may SSH to the server to check progress.  To confirm it is still running:

`ps -ef | grep 'clean package'`

You should see output like:

```
root     31761  7899  1 17:08 pts/1    00:00:06 /usr/lib/jvm/java-7-openjdk-amd64/jre/bin/java -classpath /usr/share/maven/boot/plexus-classworlds-2.x.jar -Dclassworlds.conf=/usr/share/maven/bin/m2.conf -Dmaven.home=/usr/share/maven org.codehaus.plexus.classworlds.launcher.Launcher clean package -Ddeployment=development -Dsystem=ali-dev -Daurin.dir=/etc/aurin
```
