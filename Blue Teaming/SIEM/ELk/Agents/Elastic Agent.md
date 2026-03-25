Video suggested: https://youtu.be/cGmQMsFuAvw?si=auQ43VAy8lYs0p5l&t=520

### Download Elastic Agent on Windows

- Download the elastic agent from securityOnion to host machine.

```powershell
 pipx install uploadserver
```

```bash
uploadserver
```

- Go to your python server on Edge `10.10.3.2:8000` and download file to windows.
- Run the executable as administrator.

### Install from kibana

https://discuss.elastic.co/t/insecure-flag-in-fleet-elastic-agent-deployment-to-solve-x509-certificate-signed-by-unknown-authority/363122/4

`--certificate-authorities` flag is a must even though CA cert is imported in the endpoint.

```powershell
 .\elastic-agent.exe install --url=https://fleet-server:8220 --enrollment-token=Z1FHY2I1Y0JfN3ZTLWZGZlhQdWM6Y29rcHo1NVNEdXpxR3VnME91U09QQQ== --certificate-authorities=C:\Users\marry\Documents\ca.crt
```

### Firewall config
- To enable Elastic agent to send logs, please add your subnet to the allowed lists here.
- `elasticsearch_rest` - rest API endpoint running on port `9200`. (We are directly sends logs to elastic search, skipping `logstash`)

![[Pasted image 20250606125342.png]]

![[Pasted image 20250609142211.png]]


### Verify

- **Kibana -> Management -> Fleet** or **Tools -> Elastic Fleet** on securityOnion

![[Pasted image 20250606130814.png]]

- Go to: **Kibana -> Analytics -> Discover ->** 
- Add `agent.name` and `process.name` to **Selected Fields**

![[Pasted image 20250609142704.png]]

>### ⚠️ Note
>Make sure your windows can resolve the SecurityOnion host name.
>Check with `elastic-agent status` to view any errors.
>`elastic-agent` - should be in `C:\Program Files\elastic\agent\`
>Edit hosts file in windows to include: `10.10.20.100 soc-server`. 
>`soc-server` is your SecurityOnion host name.
