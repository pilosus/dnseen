# dnseen - DNS queries analyzer

`dnseen` is a simple DNS queries analyzer.

## Install

1. Install `tcpdump`
2. Copy content of the `dnseen.service` file
3. Paste the content to a `systemd` service:

```shell
sudo -E systemctl edit dnseen --full --force
```

4. Enable the service, start and make sure it works:

```shell
sudo systemctl daemon-reload
sudo systemctl enable dnseen.service
sudo systemctl start dnseen.service 
sudo systemctl status dnseen.service 
```

5. (Optionally) Add `logrotate` config :

```shell
sudo cp dnseen.logrotate /etc/logrotate.d/dnseen
```

## Use

...
