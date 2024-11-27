# Steps to Deploy and Test

Follow these steps to deploy and test the dynamic secret reloading setup in Kubernetes.

## 1. Apply the ConfigMap for the Script

```bash
kubectl apply -f configmap.yaml
```

## 2. Create the Secret

```bash
kubectl apply -f secret.yaml
```

## 3. Deploy the Application

```bash
kubectl apply -f deployment.yaml
```

## 4. Monitor Logs

To check if the script is running correctly and reacting to changes:

```bash
kubectl logs -l app=secret-watcher
```

## 5. Update the Secret

Modify the secret to trigger an update:

```bash
echo -n "new-secret-value" | base64
kubectl edit secret my-secret
```

## 6. Check Logs for Reload

After updating the secret, verify that the changes are detected and logged:

```
$ kubectl logs -l app=secret-watcher

Monitoring /etc/secrets/my-secret for changes...
Wed Nov 27 07:10:24 UTC 2024: Setting up watch on /etc/secrets/my-secret
Setting up watches.
Watches established.
/etc/secrets/my-secret DELETE_SELF
Wed Nov 27 07:11:30 UTC 2024: Change detected, reloading secret...
Wed Nov 27 07:11:31 UTC 2024: New secret value detected: new-secret-value-2
Wed Nov 27 07:11:31 UTC 2024: Setting up watch on /etc/secrets/my-secret
Setting up watches.
Watches established.
```
