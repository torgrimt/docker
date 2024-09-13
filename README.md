# docker
Docker compose filer for diverse homelab stash.

## install via ansible
```
echo "torgrimt    ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/admin
ansible-galaxy role install -r requirements.yml
ansible-playbook install-docker.yaml
```

## Apps
All defined apps.

## Scripts
dccd - Updates containers

## Huskelapp

Finne version:
docker image inspect --format '{{json .}}' "ghcr.io/netbootxyz/netbootxyz" | jq -r '. | {Id: .Id, Digest: .Digest, RepoDigests: .RepoDigests, Labels: .Config.Labels}
