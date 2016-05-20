AWS SSH/SCP Connector
=====================
This is a small utility to help you connect to your aws instances.

Examples
--------
Some examples
```bash
eu1ssh 012345678
eu1ssh i-012345678
eu1ssh ubuntu@012345678
eu1ssh ubuntu@i-012345678
```

```bash
eu1scp 012345678:/tmp/some-file.zip /local/file/path.zip
```

Assumptions/Requirements
------------------------
It is assumed that you have installed and properly configured AWS ec2-tools. Any version that supports `ec2-describe-instances` will do fine

How does it work
----------------
Depending on the name of the script/symlink the script determines region and command to execute. After that it picks up the first command line parameter and parses it for username/path/host information.
A proper instance-id is constructed. After that the instances (filtered by that proper instance-id) are listed and the private address is collected.
An ssh config file is built and the command executed.

How to use it
-------------
As is this will *NOT* work for you. You *NEED* to make edits to make it work for you.

At the top of the script there are several variables. Change them to taste as follows:

```bash
# the bastion base name host
_bastion_base='bastion'
_bastion_end='company.com'
```

The full bastion hostname is constructed with the scheme `${_bastion_base}.${_full_region}a.${_bastion_end}`

```bash
declare -A _region_map=(
	[eu1]=eu-west-1
	[us1]=us-west-1
	[ap1]=ap-southeast-1
)
```

Declare mapping between short region name (3 letters) and full region name. The full name is used in several places so it must be a valid region name.

```bash
_personal_user='username'
```

Your username on the bastion host.

Setting it up for multiple regions
----------------------------------
In order to not duplicate the script the suggested installation is as follows:

* Clone repo
```bash
git clone https://github.com/zeridon/aws-ssh-scp-connector.git ~/toos/aws-ssh-connector
```
* Symlink to the proper names in your local/global bin directory
```bash
ln -s ~/toos/aws-ssh-connector/aws-ssh-connector ~/bin/eu1ssh
ln -s ~/toos/aws-ssh-connector/aws-ssh-connector ~/bin/eu1scp
ln -s ~/toos/aws-ssh-connector/aws-ssh-connector ~/bin/us1ssh
ln -s ~/toos/aws-ssh-connector/aws-ssh-connector ~/bin/us1scp
```

This will give you ssh/scp to the eu1 region

Other notes
-----------
* Reading the source is your best friend.
* I am using this for quite some time and it works for me. It might not for you though
* Bastion host is assumed to be in the a zone
* SCP support is more or less experimental. For single files it will work, but multiple (or speciffying additional options the way you are used to ... no)

Contributing
------------
This is released under MIT license. Feel free to use/reuse/mangle/etc. If you break it you have to keep all the pieces. If you have comments, suggestions, bugs. Feel free to open an issue or a pull request.
