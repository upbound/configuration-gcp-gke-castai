# GCP GKE CAST AI Configuration

This repository contains a reference CAST AI Configuration for
[Crossplane](https://crossplane.io). It's a great starting point for
building internal cloud platforms with autoscaling and offering a
self-service API to your internal development teams.

This platform offers APIs for setting up fully configured CAST AI
solution with Node Configuration, Node Templates, Autoscaler policies,
and all Castware components such as `castai-agent`,
`castai-cluster-controller`, `castai-evictor`, `castai-spot-handler`.
All these components are built using cloud service tools from the
[Official Upbound GCP Provider](https://marketplace.upbound.io/providers/upbound/provider-family-gcp/) and
[Partner Upbound CAST AI Provider](https://marketplace.upbound.io/providers/crossplane-contrib/crossplane-provider-castai)

## Quickstart

### Prerequisites

- CAST AI account
- CAST AI Credentials
- GCP Credentials

### Configure the CAST AI provider

Before we can use the reference configuration we need to configure it with CAST AI FullAccess API Access Key:
- Obtained CAST AI [API Access Key](https://docs.cast.ai/docs/authentication#obtaining-api-access-key)

```console
# Create Secret with CAST AI API Access Key
cat <<EOF | ${KUBECTL} apply -f -
apiVersion: v1
kind: Secret
metadata:
  name: castai-creds
  namespace: upbound-system
type: Opaque
stringData:
  credentials: |
    {
      "api_token": "y0ur-t0k3n",
      "api_url": "https://api.cast.ai"
    }
EOF

# Configure the CAST AI Provider to use the secret:
kubectl apply -f examples/castai-default-provider.yaml
```

### Configure the GCP provider

Before we can use the reference configuration we need to configure it with GCP
credentials:

```console
# Create a K8s secret with the GCP creds:
kubectl create secret generic gcp-creds -n upbound-system --from-file=credentials=./creds.json

# Configure the GCP Provider to use the secret:
kubectl apply -f examples/gcp-default-provider.yaml
```


## Using the GCP GKE CAST AI Configuration

Create the Configuration:
```console
kubectl apply -f examples/configuration.yaml
```

Create Cluster in ReadOnly mode:
```console
kubectl apply -f examples/readonly.yaml
```

🎉 Congratulations. You have just installed your CAST AI configuration!

## Overview

```
crossplane beta trace xreadonly configuration-gcp-gke-castai
NAME                                               SYNCED   READY   STATUS
XReadOnly/configuration-gcp-gke-castai             True     True    Available
├─ GkeCluster/configuration-gcp-gke-castai-4crh2   True     True    Available
└─ Release/configuration-gcp-gke-castai-dnkxz      True     True    Available

```

## Questions?

For any questions, thoughts and comments don't hesitate to [reach
out](https://www.upbound.io/contact) or drop by
[slack.crossplane.io](https://slack.crossplane.io), and say hi!
