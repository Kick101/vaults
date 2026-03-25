__The 6 vulnerability types are:__
Amazon S3 bucket allows for full anonymous access  
Amazon S3 bucket allows for arbitrary file listing  
Amazon S3 bucket allows for arbitrary file upload and exposure  
Amazon S3 bucket allows for blind uploads  
Amazon S3 bucket allows arbitrary read/writes of objects  
Amazon S3 bucket reveals ACP/ACL

---
### Enumeration
__DNS Lookup__
- dig
- nslookup (AWS Region)

__Authenticate__
```bash
aws configure --profile ctf
```

__List Bucket Contents__
- No authentication
```bash
aws s3 ls $bucket_name
```

```bash
aws s3 ls $bucket_name --no-sign-request
```
- Through their URLs
	- `http://s3.amazonaws.com/$bucket_name`
	- `http://$bucket_name.s3.amazonaws.com/`
- Authentication
```bash
aws s3 ls s3://$bucket_name --profile YOUR_ACCOUNT
```

__Sync Contents__
```bash
aws s3 sync $bucket_name --no-sign-request .
```

__Download contents__
```bash
aws s3 sync $bucket_name 
```


