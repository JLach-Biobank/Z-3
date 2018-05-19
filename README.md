projekt Archiwizacja danych

```
docker build . -t harbour:latest
```

```
export AWS_ACCESS_KEY_ID=[key];
export AWS_SECRET_ACCESS_KEY=[key];
docker run -e AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID -e AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY -e AWS_DEFAULT_REGION=us-east-1 harbour:latest s3 ls
```

```
#upload
docker run -v "$PWD/data/":/tmp/data/ -e AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID -e AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY -e AWS_DEFAULT_REGION=us-east-1 harbour:latest s3api put-object --bucket biohack-bucket --key 1.vcf --body /tmp/data/1.vcf
```

```
#list-object
docker run -v "$PWD/data/":/tmp/data/ -e AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID -e AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY -e AWS_DEFAULT_REGION=us-east-1 harbour:latest s3api list-objects --bucket biohack-bucket
```

```
#get-object-tagging
```

docker run -e AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID -e AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY -e AWS_DEFAULT_REGION=us-east-1 harbour:latest s3api get-object-tagging --bucket biohack-bucket --key 1.vcf

```
#put-object-tagging (OVERRIDE)
```

a) tag-value purely in command

docker run -e AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID -e AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY -e AWS_DEFAULT_REGION=us-east-1 harbour:latest s3api put-object-tagging --bucket biohack-bucket --key 1.vcf --tagging '{ "TagSet": [{"Key": "test_foo","Value": "foo_1"},{"Key": "test_bar","Value": "bar_1"}]}'

b) tag-value from file 

echo '{"TagSet": [{"Key": "test_foo","Value": "foo"},{    "Key": "test_bar","Value": "bar"}]}' > tag-example-file-1.json
docker run -e AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID -e AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY -e AWS_DEFAULT_REGION=us-east-1 harbour:latest s3api put-object-tagging --bucket biohack-bucket --key 1.vcf --tagging "$(<tag-example-file-1.json)"


