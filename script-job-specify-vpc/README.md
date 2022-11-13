# create vpc

```
gcloud compute networks create test-vpc \
  --project=ornate-spring-351002 \
  --description=VPC\ temporary \
  --subnet-mode=custom \
  --mtu=1460 \
  --bgp-routing-mode=regional
```

# create subnet

```
gcloud compute networks subnets create test-subnet-usc1
  --project=ornate-spring-351002 \
  --description=subnet\ temporary \
  --range=10.0.0.0/24 \
  --stack-type=IPV4_ONLY \
  --network=test-vpc \
  --region=asia-northeast1 \
  --enable-private-ip-google-access
```

# create batch job

```
gcloud batch jobs submit job-vpc \
  --location asia-northeast1 \
  --config job_config.json
```
