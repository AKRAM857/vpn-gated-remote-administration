## 01 — SCP File Transfer Direction

### Problem
Attempted to copy `publickey` from the local machine to `192.168.10.33`, but `scp` returned:

`scp: /vpn_project_keys/publickey: No such file or directory`

### What We Wanted
Transfer:

`/root/vpn_project_keys/publickey`

from the local machine to:

`ansible_user@192.168.10.33:/root/vpn_project_keys/`

### Why It Failed
`scp` follows the syntax:

`scp SOURCE DESTINATION`

The remote path was placed first, so `scp` interpreted it as the **source** and tried to retrieve the file from `192.168.10.33`.

The `-i` option only specifies the SSH private key used for authentication; it does not define the file being transferred.

### Fix
```bash
scp -i /root/.ssh/id_ed25519 /root/vpn_project_keys/publickey ansible_user@192.168.10.33:/root/vpn_project_keys/
