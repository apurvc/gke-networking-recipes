    4  gcloud container clusters create test-ilb     --zone=europe-west1-b --enable-ip-alias --release-channel rapid
    5  gcloud container clusters get-credentials  test-ilb     --zone=europe-west1-b
    7  alias k=kubectl
    8  cd gke-networking-recipes/ingress/single-cluster/ingress-internal-basic/
    9  k apply -f internal-ingress-basic.yaml
    10  k get ing
    13  openssl req -x509 -newkey rsa:4096 -sha256 -days 3650 -nodes   -keyout example.key -out example.crt -subj "/CN=foo.example.com"
    17  openssl req -x509 -newkey rsa:2048 -sha256 -days 3650 -nodes   -keyout example.key -out example.crt -subj "/CN=foo.example.com"
    18  gcloud compute ssl-certificates create foocrt     --certificate example.crt     --private-key example.key     --region europe-west1
    19  k apply -f internal-ingress-https.yaml
   
   Test
   
   curl -v -k https://foo.example.com --resolve foo.example.com:443:[10.132.0.8] LBIP 
