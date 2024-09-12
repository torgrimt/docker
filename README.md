# docker
Docker compose filer for diverse homelab stash.

## install via ansible
```
echo "torgrimt    ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/admin
ansible-galaxy role install -r requirements.yml
ansible-playbook install-docker.yaml
```

## Apps
her ligger alle apper...

## Scripts
Scripts for diverse..


## Huskelapp

Finne version:
docker image inspect --format '{{json .}}' "ghcr.io/netbootxyz/netbootxyz" | jq -r '. | {Id: .Id, Digest: .Digest, RepoDigests: .RepoDigests, Labels: .Config.Labels}
