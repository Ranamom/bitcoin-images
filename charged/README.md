## Charged
The Dockerfile was adapted from https://github.com/ElementsProject/lightning-charge/blob/master/Dockerfile.

Feel free to adapt the `build-and-push-to-dockerhub.sh` to push to your own repo/registry.

### How to run
```
/etc/systemd/system/charge.service
[Unit]
Description=Charge daemon
Wants=docker.target
After=docker.service

[Service]
Restart=always
RestartSec=3
ExecStartPre=/usr/bin/docker pull blockstream/charged:latest
ExecStart=/usr/bin/docker run \
    --network=host \
    --pid=host \
    --name=charge \
    -v /mnt/data/lightning:/root/.lightning:ro \
    -v /mnt/data/charge:/data:rw \
    -e "API_TOKEN=$TOKEN"
    blockstream/charged:latest charged -d /data/charge.db -l /root/.lightning
ExecStop=/usr/bin/docker rm -f charge
```
