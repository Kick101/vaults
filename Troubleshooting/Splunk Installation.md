## On Arch

- Create user splunk and give permission to dir
```shell
sudo useradd -m splunk
sudo chown -R splunk:splunk /opt/splunk
```

```shell
sudo tar -xvzf splunk*.tgz -C /opt
sudo -u splunk -i
cd /opt/splunk
./bin/splunk start --accept-license
```

