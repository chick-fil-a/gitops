# KubeCon + Cloud Native Con Setup

Setup guide for Chick-fil-A GitOps demo.

## DNS

Add local [host](/etc/hosts) settings for demo of cloud side services.

```
##
# Host Database
#
# localhost is used to configure the loopback interface
# when the system is booting.  Do not change this entry.
##
127.0.0.1	gitlab.cloud
127.0.0.1	vault.cloud
```

## Gitlab

Start gitlab:

```bash
docker run --rm \
  --name gitlab \
  --hostname gitlab.cloud \
  --env GITLAB_OMNIBUS_CONFIG="external_url 'http://gitlab.cloud/'" \
  --publish 443:443 --publish 80:80 --publish 22:22 \
  --volume $PWD/gitlab/etc:/etc/gitlab \
  --volume $PWD/gitlab/log:/var/log/gitlab \
  --volume $PWD/gitlab/opt:/var/opt/gitlab \
  gitlab/gitlab-ce:11.5.3-ce.0
```

Browse to http://gitlab.cloud (assuming etc/hosts entries entered above) and login using:

```
Username: root
Password: eatmorchikin
```

### Vault

```bash
docker run --rm \
  --name vault \
  --cap-add IPC_LOCK \
  --publish 8200:8200 \
  vault:1.0.0

# get root token from output of above command
vault login <Root Token>

# store sample secret
cat notsosecret.yaml | vault kv put secret/atlas/kubecon.cluster.riot.edge/podinfo/secret.yaml spec=-
cat notsosecret.yaml | vault kv put secret/atlas/atlanta.cluster.riot.edge/podinfo/secret.yaml spec=-

```

Brose to http://vault.cloud:8200 and login using :

```
Root Token: <root token from vault startup>
```

## Weave Cloud

Create a [Weave Cloud account](https://cloud.weave.works/account)

Base 64 encode the token and place into the [secret file](weave/secret.yaml)

```bash
kubectl apply -f weave/namespace.yaml
kubectl apply -f weave
```
