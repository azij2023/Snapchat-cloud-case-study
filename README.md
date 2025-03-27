# Snapchat-cloud-case-study
apiVersion: apps/v1
kind: Deployment
metadata:
  name: media-processing-service
spec:
  replicas: 3
  selector:
    matchLabels:
      app: media-processing
  template:
    metadata:
      labels:
        app: media-processing
    spec:
      containers:
      - name: media-processor
        image: gcr.io/snapchat/media-processor:latest
        ports:
        - containerPort: 8080
        resources:
          requests:
            memory: "512Mi"
            cpu: "500m"
          limits:
            memory: "1Gi"
            cpu: "1"

# Terraform Configuration for GKE Cluster:
provider "google" {
  project = "snapchat-project"
  region  = "us-central1"
}

resource "google_container_cluster" "primary" {
  name     = "snapchat-cluster"
  location = "us-central1"

  node_config {
    machine_type = "e2-standard-4"
    oauth_scopes = [
      "https://www.googleapis.com/auth/cloud-platform",
    ]
  }

  initial_node_count = 3
}
