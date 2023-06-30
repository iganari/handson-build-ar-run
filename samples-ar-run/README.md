# Cloud Build ---> Cloud Run のサンプル( Artifact Registry )

## 概要

ほげほげ

図も書く

## やってみる

### 0. 準備

```
export _gc_pj_id='Your Google Cloud Project ID'
export _common='hnd-cb-sample'
```

+ Google Cloud と認証をする

```
gcloud auth login --no-launch-browser
```

+ ローカルの Docker の設定をアップデートする
  + https://cloud.google.com/artifact-registry/docs/docker/authentication

```
gcloud auth configure-docker asia-northeast1-docker.pkg.dev
```

+ ソースコードを clone する

```
git clone https://github.com/iganari/handson-cloudbuild.git
cd handson-cloudbuild
```

### 1. Service Account の作成と Role の付与

+ Service Account の作成

```
gcloud beta iam service-accounts create sa-${_common}-cloudbuild \
  --description="Cloud Build Trigger 毎に Service Account を付与する" \
  --display-name="sa-${_common}-cloudbuild" \
  --project ${_gc_pj_id}
```

+ Service Account の確認
  + Logs Writer( `roles/logging.logWriter` )
  + Storage Admin( `roles/storage.admin` )
  + Artifact Registry Writer( `roles/artifactregistry.writer` )
  + Cloud Run Developer( `roles/run.developer` )
  + Service Account User( `roles/iam.serviceAccountUser` ) 



```
gcloud beta iam service-accounts describe sa-${_common}-cloudbuild@${_gc_pj_id}.iam.gserviceaccount.com --project ${_gc_pj_id} --format json
```

+ Role を付与する

```
### Logs Writer
gcloud beta projects add-iam-policy-binding ${_gc_pj_id} \
  --member="serviceAccount:sa-${_common}-cloudbuild@${_gc_pj_id}.iam.gserviceaccount.com" \
  --role="roles/logging.logWriter" \
  --project ${_gc_pj_id}

### Storage Admin
gcloud beta projects add-iam-policy-binding ${_gc_pj_id} \
  --member="serviceAccount:sa-${_common}-cloudbuild@${_gc_pj_id}.iam.gserviceaccount.com" \
  --role="roles/storage.admin" \
  --project ${_gc_pj_id}

### Artifact Registry Writer
gcloud beta projects add-iam-policy-binding ${_gc_pj_id} \
  --member="serviceAccount:sa-${_common}-cloudbuild@${_gc_pj_id}.iam.gserviceaccount.com" \
  --role="roles/artifactregistry.writer" \
  --project ${_gc_pj_id}

### Cloud Run Developer
gcloud beta projects add-iam-policy-binding ${_gc_pj_id} \
  --member="serviceAccount:sa-${_common}-cloudbuild@${_gc_pj_id}.iam.gserviceaccount.com" \
  --role="roles/run.developer" \
  --project ${_gc_pj_id}

### Service Account User
gcloud beta projects add-iam-policy-binding ${_gc_pj_id} \
  --member="serviceAccount:sa-${_common}-cloudbuild@${_gc_pj_id}.iam.gserviceaccount.com" \
  --role="roles/iam.serviceAccountUser" \
  --project ${_gc_pj_id}
```












### 1. hoge

Google Cloud と GitHub を連携する

```
### GitHub リポジトリに接続する
https://cloud.google.com/build/docs/automating-builds/github/connect-repo-github
```