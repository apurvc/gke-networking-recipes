    Create proxy only subnet for GCP ingress and a Firewall to allow connections from Internal HTTPS LB(IP) to pod port. Ref https://cloud.google.com/kubernetes-engine/docs/how-to/internal-load-balance-ingress

    gcloud container clusters create test-ilb     --zone=europe-west1-b --enable-ip-alias --release-channel rapid
    gcloud container clusters get-credentials  test-ilb     --zone=europe-west1-b
    alias k=kubectl
    git clone https://github.com/apurvc/gke-networking-recipes.git
    cd gke-networking-recipes/ingress/single-cluster/ingress-internal-basic/
    k apply -f internal-ingress-basic.yaml
    k get ing
 ### Self Signed certificate   
 
    openssl req -x509 -newkey rsa:2048 -sha256 -days 3650 -nodes   -keyout example.key -out example.crt -subj "/CN=foo.example.com"
    gcloud compute ssl-certificates create foocrt     --certificate example.crt     --private-key example.key     --region europe-west1
    k apply -f internal-ingress-https.yaml
   
### Test
   
   curl -v -k https://foo.example.com --resolve foo.example.com:443:[10.132.0.8] LBIP 

### Self signed cert served from kube
   Deploy bitnami nginx ingress controller then add update the service via values.yaml
     annotations: {
    cloud.google.com/load-balancer-type: "Internal",
    networking.gke.io/internal-load-balancer-allow-global-access: "true"
  }

   kubectl create secret tls foo-exmple-tls --key="example.key" --cert="example.crt"
   k apply -f ingress-localtls.yaml

   Reference the secret in Ingress definitation 
