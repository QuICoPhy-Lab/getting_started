# SSH quick guide

Use SSH keys to avoid typing your password at every connection.

## Setup

1. Generate a key pair on your local machine (`ssh-keygen`).
2. Add the public key to your [cluster account](https://ccdb.alliancecan.ca/security/login).
3. Customize the `config` file in this folder and copy it to `~/.ssh/config`.

For a more detailed tutorial, please visit [Using SSH keys in Linux](https://docs.alliancecan.ca/wiki/Using_SSH_keys_in_Linux#Creating_a_key_pair).

## Connect using aliases

After setup, connect with short commands such as:

```bash
ssh iq
ssh narval
```

Make sure you replace placeholder values (especially `CIP` and key paths) with your own information.
