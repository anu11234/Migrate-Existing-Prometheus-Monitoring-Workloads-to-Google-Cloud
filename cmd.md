# Migrate Existing Prometheus Monitoring Workloads to Google Cloud || **GSP1025**

**Command:**

```bash
ZONE="<ZONE>"
gcloud config set compute/zone $ZONE && \
gcloud container clusters create gmp-cluster --num-nodes=3 --zone=$ZONE || gcloud container clusters get-credentials gmp-cluster --zone=$ZONE && \
gcloud container clusters get-credentials gmp-cluster --zone=$ZONE && \
kubectl create ns gmp-test --dry-run=client -o yaml | kubectl apply -f - && \
kubectl -n gmp-test apply -f https://raw.githubusercontent.com/GoogleCloudPlatform/prometheus-engine/v0.4.3-gke.0/examples/example-app.yaml && \
kubectl -n gmp-test apply -f https://raw.githubusercontent.com/GoogleCloudPlatform/prometheus-engine/v0.4.3-gke.0/examples/prometheus.yaml && \
export PROJECT_ID=$(gcloud config get-value project) && \
curl -s https://raw.githubusercontent.com/GoogleCloudPlatform/prometheus-engine/v0.4.3-gke.0/examples/frontend.yaml | sed "s/\$PROJECT_ID/$PROJECT_ID/" | kubectl apply -n gmp-test -f - && \
kubectl -n gmp-test apply -f https://raw.githubusercontent.com/GoogleCloudPlatform/prometheus-engine/v0.4.3-gke.0/examples/grafana.yaml
