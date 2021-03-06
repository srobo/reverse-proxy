# Deploying the Proxy

This assumes you've got access to a kubernetes cluster. We have [a guide][guide]
on how we do this at Student Robotics.

## Provisioning from Scratch

### DigitalOcean

To host the official reverse-proxy, we use DigitalOcean. To access the
kubernetes cluster, you need to have access to our DigitalOcean account. To
setup your `kubectl` for the following steps, you need to do the following:

1. Log into DigitalOcean, and into the StudentRobotics project.

2. Go to [API > Generate New Token][tokens]

3. Download [doctl][doctl]

4. Log into doctl and, when prompted, enter your access token.

   ```bash
   doctl auth init
   ```

5. Pick a cluster you want to log into.

   ```bash
   doctl k8s cluster list
   ```

5. Set your kubectl context to the digitalocean cluster

   ```bash
   doctl k8s cluster kubeconfig save $CLUSTER_NAME
   ```

   This should make your `kubectl` command point to the right place.

### Kubernetes

To deploy your application to the cluster, and expose it on the host
container under port `30000`, you need to provision it on kubernetes.

```bash
kubectl create -f kubernetes/
```

## Updating an Existing Deployment

Because we don't do any sophisticated tagging, we have to force Kubernetes to
pull the latest version of the app. We do this by doing the following:

```bash
kubectl patch deployment reverse-proxy \
    -p "{\"spec\":{\"template\":{\"metadata\":{\"labels\":{\"date\":\"`date +'%s'`\"}}}}}"
```

If you've changed any of the kubernetes configuration, you need to also deploy
to kubernetes.

```bash
kubectl apply -f kubernetes/
```

[tokens]: https://cloud.digitalocean.com/account/api/tokens
[doctl]: https://github.com/digitalocean/doctl
[guide]: https://github.com/srobo/infrastructure#interacting-with-kubernetes
