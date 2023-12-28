# dnseen - DNS queries analyzer

`dnseen` is a simple DNS queries analyzer that works on top of the `tcpdump` logs.

- Simple: no GUI/TUI, no modes, easy command-line interface
- Stands on the shoulders of giants: used `tcpdump` and `systemd` at
  its core
- Separation of concerns: logs and a stats report produced by
  different components and can be used independently
- Filtering: select a datetime range, filter out domains with regex,
  filter by domain hits, etc.

## Install

1. Clone the repo and `cd` to it:

```shell
git clone https://github.com/pilosus/dnseen.git
cd dnseen
```

2. Install dependencies:

- `tcpdump`
- [babashka](https://github.com/babashka/babashka#installation)
- (optionally) `logrotate`

3. Copy content of the `dnseen.service` file and paste to a new
   `systemd` service:

```shell
sudo -E systemctl edit dnseen --full --force
```

Alternatively, simply copy the service file:

```shell
sudo cp dnseen.service /etc/systemd/system/
```

4. Reload `systemd`, start the service, enable it to start
   automatically on system boot, and make sure it works:

```shell
sudo systemctl daemon-reload
sudo systemctl start dnseen.service 
sudo systemctl enable dnseen.service
sudo systemctl status dnseen.service 
```

5. (Optionally) Add `logrotate` config file to make logs rotated:

```shell
sudo cp dnseen.logrotate /etc/logrotate.d/dnseen
```

Make sure config is valid:

```shell
sudo logrotate --debug /etc/logrotate.d/dnseen
```

If needed, force rotation:

```shell
sudo logrotate --force /etc/logrotate.d/dnseen
```

## Use

Basic usage takes the whole log and prints the report without any
filters applied, domains ordered by number of hits in descending
order:

```shell
./dnseen
```

Apply some filters if needed:

```shell
./dnseen \
    --from "2023-12-01T00:00:00" \
    --to "2023-12-08T00:00:00" \
    --exclude '(?i).*dnsleaktest' \
    --hits 10
```

Find more options in help:

```shell
./dnseen --help
```
