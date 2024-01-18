# Back up AWX using AWX Operator

The AWX Operator `0.10.0` or later has the ability to back up AWX in easy way.

You can also refer [the official instructions](https://github.com/ansible/awx-operator/tree/devel/roles/backup) for more information.

## Table of Contents

- [Back up AWX using AWX Operator](#back-up-awx-using-awx-operator)
  - [Table of Contents](#table-of-contents)
  - [Instruction](#instruction)
    - [Back up AWX manually](#back-up-awx-manually)
  - [Appendix: Back up AWX using Ansible](#appendix-back-up-awx-using-ansible)

## Instruction

### Back up AWX manually

Modify the name of the AWXBackup object in `backup/backup-awx.yml`.

```yaml
...
kind: AWXBackup
metadata:
  name: awx-backup-2024-1-4     <--
  namespace: awx
...
```

Then invoke backup by applying this manifest file.

```bash
kubectl apply -f backup/backup-awx.yml
```

To monitor the progress of the deployment, check the logs of `deployments/awx-operator-controller-manager`:

```bash
kubectl -n awx logs -f deployments/awx-operator-controller-manager
```

When the backup completes successfully, the logs end with:

```txt
$ kubectl -n awx logs -f deployments/awx-operator-controller-manager
...
----- Ansible Task Status Event StdOut (awx.ansible.com/v1beta1, Kind=AWXBackup, awx-backup-2024-1-4/awx) -----
PLAY RECAP *********************************************************************
localhost                  : ok=7    changed=0    unreachable=0    failed=0    skipped=9    rescued=0    ignored=0
```

This will create AWXBackup object in the namespace and also create backup files in the Persistent Volume. In this example those files are available at `/data/backup`.

```bash
$ kubectl -n awx get awxbackup
NAME                   AGE
awxbackup-2021-06-06   6m47s
```

```bash
$ ls -l /data/backup/
total 0
drwxr-xr-x. 2 root root 59 Jun  5 06:51 tower-openshift-backup-2021-06-06-105149

$ ls -l /data/backup/tower-openshift-backup-2021-06-06-105149/
total 736
-rw-------. 1 1001 root   1093 Jun  6 06:51 awx_object
-rw-------. 1 1001 root  17085 Jun  6 06:51 secrets.yml
-rw-rw----. 1 root root 833184 Jun  6 06:51 tower.db
```

## Appendix: Back up AWX using Ansible

An example simple playbook for Ansible is also provided in this repository. This can be used with `ansible-playbook`, `ansible-runner`, and AWX. It can be also used with the scheduling feature on AWX too.

Refer [📁 **Appendix: Back up AWX using Ansible**](ansible) for details.
