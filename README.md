# docker
Poor mans docker compose ci/cd system.
Could not find what i was looking for, so i hacked it together with some ductape and string.

The idea is to have small intel nucs running different places, that share a codebase in apps controlled by its own .env file.
Renovatebot runs and updates the imagetags, and dccd is used to pull from git and run docker-compose to update them.
It used traefik as an "ingress" to proxy all the apps if needed. Controlled by a label.

Some of the apps templates are stolen from [Christian Lempa boilerplate](https://github.com/ChristianLempa/boilerplates/tree/main/docker-compose) repository, and modified to my needs.
And its using a modified version of [dccd](https://github.com/loganmarchione/dccd) thats using docker-compose --dry-run to only restart containers that needs too be.
The srccode for my version you can find [here](https://github.com/torgrimt/dccd)

It requires atleast git and docker to be installed.

## install docker via ansible
If no ansible server is setup, you can use the following to pull and install docker from the official repos.
```
echo "username    ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/admin
ansible-galaxy role install -r requirements.yml
ansible-playbook install-docker.yaml
```

## Usage:
Here is an example of getting traefik and homepage to run on a node.
Probably a good idea to fork it, so you can modify the repo...
```
git clone https://github.com/torgrimt/docker.git
cd docker/apps/traefik
docker compose up -d

cd ../homepage/
# Fetch the vars from the composefile
grep VAR docker-compose.yaml | awk -F "\$" '{print $2}'| sed 's/)//;s/"//;s/`//' > .env
# Edit the .env file to match your needs.

# Update the local link, and start it up.
cd
bash docker/scripts/linklocal-docker
cd docker/local/homepage
docker compose up -d
```

## Updating
Whenever you have updated the gitrepo or renovatebot have updated version numbers, you can run the script update-containers
and it will pull the gitrepo, and update the files and restart containers that needs it. Automagick
It is intended to run as a cronjob.

## Backup
The included backupscript "backup-docker", will make some "smart" backups of only the userdata that is defined for the containers.
It will end up in /home/yourname/backup so another host can rsync them if needed.
This is intended to run as a cronjob.

## Apps
All defined apps.

## Included Scripts
|Script         |Description|
|---------      |-----------------------------------------------|
|dccd.sh| Updates containers and restarts them. Should be used from update-containers|
|linklocal-docker|Makes links for local running containers|
|backup-docker|takes a backup of the userdata|
|dnsnames|Finds and test resolve hostnames configured for traefik|
|show-docker-vars|Finds all you local variables|
|update-containers| script to pull and update containers|

## Handy commands:
docker image inspect --format '{{json .}}' "ghcr.io/netbootxyz/netbootxyz" | jq -r '. | {Id: .Id, Digest: .Digest, RepoDigests: .RepoDigests, Labels: .Config.Labels}'


