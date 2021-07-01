kubectl logs --tail=-1 -f -l name=sealed-secrets-controller -n kube-system

echo -n 'KeySecretToken' | base64

kubeseal --fetch-cert > cert.pem

openssl x509 -in cert.pem -text -noout

kubeseal < ddclient-secret.yaml --cert cert.pem -o yaml > ddclient-sealedsecret.yaml

