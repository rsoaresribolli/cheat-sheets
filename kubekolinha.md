# AWS 

## Authentication with MFA

1. First, set the profile you want to use
```bash
export AWS_PROFILE=<profile_name>
```
2. Then, run 
```bash
aws-mfa
```

# Configurations

## Apply

Apply file configurations
```bash
kubectl apply -f .\deployment.yaml -f .\ingress.yaml -f .\service.yaml
```


# Management
## Get pods and namespaces

```bash
kubectl get ns
kubectl -n <namespace> get pods
```

### Tip: copy pod name to clipboard
```bash
 kubectl -n <namespace> get pods | awk 'NR==2{print $1}' > /dev/clipboard
```

## Get into pod

```bash
kubectl -n <namespace> exec -it <pod_name> -- bash
kubectl attach psql-client -c psql-client -i -t
```

## View pod logs

```bash
kubectl -n <namespace> logs -f <pod_name>
kubectl -n <namespace> logs --follow <pod_name>
```



## Delete pod

```bash
kubectl -n <namespace> delete pod <pod_name>
```

# Secrets

## View secrets
```bash
kubectl -n <namespace> get secrets
```

## View contents of secret
To view all keys and values inside the secret
```bash
kubectl -n <namespace> get secret <secret_name> -o jsonpath='{.data}'
```
or, to view a specific key value
```bash
kubectl -n <namespace> get secret <secret_name> -o jsonpath='{.data.KEY_NAME}'
```
### Tip: decode a secret value
```bash
echo 'encoded_secret' | base64 --decode
```
or, for a specific key value
```bash
kubectl -n <namespace> get secret <secret_name> -o jsonpath='{.data.KEY_NAME}' | base64 --decode
```

## Delete secrets
```bash
kubectl delete secret <secret_name>
```

## Create secrets
1. From file
```bash
kubectl create secret generic <secret-name> --from-file=./username.txt --from-file=./password.txt
```
1.1. From file, specifying secret key
```bash
kubectl create secret generic <secret-name> --from-file=username=./username.txt --from-file=password=./password.txt
```
2. From literal
```bash
kubectl create secret generic <secret-name> --from-literal=username=devuser --from-literal=password='S!B\*d$zDsb='
```

3. From yaml
**secret.yaml**
```yaml
apiVersion: v1
kind: Secret
metadata:
 name: secret-name
type: Opaque
stringData:
 VAR1: VALUE1
 VAR2: VALUE2
```

```bash
kubectl apply -f secret.yaml
```
