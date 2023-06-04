# Ansible TerraMaster

Manage a [TerraMaster][terra-master] NAS device with [Ansible][ansible] over SSH.

This has only been tested against a [TerraMaster F5-422][f5-422].
You risk damaging your device and data here,
so please do not attempt anything without careful consideration.

## Pre-requisites

* Install Ansible
* Clone this repository
* Connect over SSH and edit the `/etc/ssh/sshd_config` to update the `AllowUsers` to include your unique user

    ```plaintext
    AllowUsers admin myuser
    ```

* Add a public key to `~/.ssh/authorized_keys` (disable SSH password authentication in `/etc/ssh/sshd_config` if you like)
* Change the permissions on `/dev/null` to `666` to prevent an Ansible failure

    ```console
    $ sudo chmod 666 /dev/null
    $ ls -l /dev/null
    crw-rw-rw- 1 root root 1, 3 Oct 10 20:19 /dev/null
    ```

## Usage

1. Rename the `inventory.example` file to `inventory.ini`
1. Update the values of `ansible_host`, `ansible_port`, `ansible_ssh_private_key_file`, and `ansible_user` within `inventory.ini` as appropriate
1. Run the `example-playbook.yml` playbook to confirm that Ansible is connecting without error to your TNAS (TerraMaster NAS).

The `-K` flag will prompt for the *root* user's password

```console
$ ansible-playbook -i ./inventory.ini -K ./example-playbook.yml
BECOME password:

PLAY [TerraMaster] ************************************************************************************************************

TASK [Gathering Facts] ********************************************************************************************************
ok: [F5-422]

TASK [ping] *******************************************************************************************************************
ok: [F5-422]

PLAY RECAP ********************************************************************************************************************
F5-422                     : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

As the TerraMaster does not use a standard Linux distribution,
there are many Ansible modules that will likely not work as you expect.

[ansible]: https://www.ansible.com/ "Ansible home page"
[terra-master]: https://www.terra-master.com/global/ "TerraMaster home page"
[f5-422]: https://www.terra-master.com/global/products/smallmedium-businesses-nas/f5-422-10g-nas.html "TerraMaster F5-422 product page"
