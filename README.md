# docker
Poor mans docker compose ci/cd system.
Could not find what i was looking for, so i hacked it together with some ductape and string.

The idea is to have small intel nucs running different places, that share a codebase in apps controlled by its own .env file.
Renovatebot runs and updates the imagetags, and dccd is used to pull from git and run docker-compose to update them.

Usage:
git pull github.com/torgrimt/docker.git

## install docker via ansible
If no ansible server is setup, you can use the following to pull and install docker from the official repos.
```
echo "username    ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/admin
ansible-galaxy role install -r requirements.yml
ansible-playbook install-docker.yaml
```

## Apps
All defined apps.

## Scripts
|Script         |Description|
|---------      |-----------------------------------------------|
|dccd| Updates containers|
|linklocal-docker|Makes links for local running containers|

## Handy commands:
docker image inspect --format '{{json .}}' "ghcr.io/netbootxyz/netbootxyz" | jq -r '. | {Id: .Id, Digest: .Digest, RepoDigests: .RepoDigests, Labels: .Config.Labels}


