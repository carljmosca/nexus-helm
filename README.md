# Sonatype Nexus Helm Chart

## These instructions assume access to Helm and an available Kubernetes cluster that has the Nginx ingress installed.

TODO: add Kubernetes and Nginx ingress installation instructions.

### Tailscale (using DNS and HTTPS certificate)

The values-tailscale-example.yaml file should be copied to values.yaml.  Next, modify the two occurennces of "some.host.ts.net" to match your Tailscale network device name (for ingress.hosts.host and ingress.tls.hosts).

#### macOS setup

The following commands set two needed environment variables, issue the tailscale create certificate command, and then the kubectl command to create the TLS secret using the Tailscale certificate.
The Tailscale network device name from above should be used for the TAILSCALE_HOSTNAME value:
```
export TAILSCALE_HOSTNAME=some.host.ts.net
export TAILSCALE_CERTIFICATE_DIRECTORY=$HOME/Library/Containers/io.tailscale.ipn.macos/Data
/Applications/Tailscale.app/Contents/MacOS/Tailscale cert $TAILSCALE_HOSTNAME
kubectl create secret tls nexus-tls-secret \
    --cert=$TAILSCALE_CERTIFICATE_DIRECTORY/$TAILSCALE_HOSTNAME.crt \
    --key=$TAILSCALE_CERTIFICATE_DIRECTORY/$TAILSCALE_HOSTNAME.key \
    --namespace=nexus
```

#### Windows setup

TODO: Windows instructions

#### Linux setup

TODO: Linux instructions

### Helm chart installation

To install or upgrade Nexus via Helm use the following command replacing the value for the global.nexusDataDirectory if/as desired:

```
 helm upgrade -f values.yaml --set global.nexusDataDirectory=$HOME/data/nexus \
    --install nexus nexus --namespace=nexus --create-namespace
```

After a couple of minutes, once the application is fully deployed (upon initial installation), a file named "admin.password" should be created in the directory specified for global.nexusDataDirectory. This value should be used to login as the admin user.

Happy Nexus-ing!!