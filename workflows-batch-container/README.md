# Artifact Registry、Batch、Cloud Build、Workflow Executions、Workflows API を有効にします

```
gcloud services enable artifactregistry.googleapis.com \
  batch.googleapis.com \
  cloudbuild.googleapis.com \
  workflowexecutions.googleapis.com \
  workflows.googleapis.com
```

# サービス アカウントを作成

```
gcloud iam service-accounts create sa-test-batch-workflows

```

# ロール付与

```
gcloud projects add-iam-policy-binding ornate-spring-351002 \
  --member="serviceAccount:sa-test-batch-workflows@ornate-spring-351002.iam.gserviceaccount.com" \
  --role=roles/batch.jobsAdmin

gcloud projects add-iam-policy-binding ornate-spring-351002 \
  --member="serviceAccount:sa-test-batch-workflows@ornate-spring-351002.iam.gserviceaccount.com" \
  --role=roles/logging.logWriter

gcloud projects add-iam-policy-binding ornate-spring-351002 \
  --member="serviceAccount:sa-test-batch-workflows@ornate-spring-351002.iam.gserviceaccount.com" \
  --role=roles/storage.admin
```

# サービス アカウントを使用する Batch ジョブを作成するためのアクセス権を自分に付与

```
gcloud projects add-iam-policy-binding ornate-spring-351002 --member \
 user:"k.sato.ontology@gmail.com" \
 --role "roles/iam.serviceAccountUser"
```

# Artifact Registry レポジトリ作成

```
gcloud artifacts repositories create containers \
    --repository-format=docker \
    --location=us-central1
```

# Download sample code

```
git clone https://github.com/GoogleCloudPlatform/batch-samples.git
```

# イメージのビルド

```
gcloud builds submit \
  -t us-central1-docker.pkg.dev/ornate-spring-351002/containers/primegen-service:v1 PrimeGenService/
```

# ワークフローのソースファイルを作成(batch-workflow.yaml)後、デプロイ

```
gcloud workflows deploy batch-workflow \
  --source=batch-workflow.yaml \
  --service-account=sa-test-batch-workflows@ornate-spring-351002.iam.gserviceaccount.com \
  --location us-central1
```

# ワークフローの実行

```
gcloud workflows run batch-workflow --location us-central1
```
