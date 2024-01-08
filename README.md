# dnseen - DNS queries analyzer

`dnseen` is a simple DNS queries analyzer that works on top of the `tcpdump` logs.

- Simple: no GUI/TUI, no modes, easy command-line interface
- Stands on the shoulders of giants: used `tcpdump` and `systemd` at
  its core
- Separation of concerns: logs and a stats report produced by
  different components and can be used independently
- Filtering: select a datetime range, filter out domains with regex,
  filter by domain hits, etc.

```
$ dnseen
|                                     :domain | :hits |
|---------------------------------------------+-------|
|                                  github.com |    87 |
|                              api.github.com |    83 |
|                           fedoraproject.org |    80 |
|                profile.accounts.firefox.com |    60 |
|                 safebrowsing.googleapis.com |    37 |
|                        collector.github.com |    36 |
|                            alive.github.com |    32 |
|                contile.services.mozilla.com |    27 |
| sync-1-us-west1-g.sync.services.mozilla.com |    22 |
|                               ocsp.pki.goog |    21 |
...
```

## Install

### Dependencies

`dnseen` requires the following dependencies:

- Linux OS
- `tcpdump`
- [babashka](https://github.com/babashka/babashka#installation)
- (optionally) `logrotate`


### Installer script

Install `dnseen` with the installer script on Linux:

```shell
curl -sLO https://raw.githubusercontent.com/pilosus/dnseen/master/install
chmod +x install
./install
```

By default, the command will be installed in `/usr/local/bin` (you may
need to use `sudo` to run the installer script in this case!). You can
change installation directory with the option `--install-dir`:

```shell
./install --install-dir <your-dir-under-$PATH>
```

To install a specific version instead of the latest one use
`--version` option:

```shell
./install --version 0.2.0
```

Installer script downloads a package archive file to a temporary
directory under `/tmp`, you can change it with the option
`--download-dir`:

```shell
./install --download-dir <your-dir-under-$PATH>
```

You can uninstall `dnseen` and all its corresponding services with the
`--uninstall` option (can be used along with `--install-dir`):

```shell
./install --uninstall
```

For more options see installer script's help:

```shell
./install --help
```

### Manual install

1. Clone the repo and `cd` to it:

```shell
git clone https://github.com/pilosus/dnseen.git
cd dnseen
```

2. Copy content of the `dnseen.service` file and paste to a new
   `systemd` service:

```shell
sudo -E systemctl edit dnseen --full --force
```

Alternatively, simply copy the service file:

```shell
sudo cp dnseen.service /etc/systemd/system/
```

3. Reload `systemd`, start the service, enable it to start
   automatically on system boot, and make sure it works:

```shell
sudo systemctl daemon-reload
sudo systemctl start dnseen.service 
sudo systemctl enable dnseen.service
sudo systemctl status dnseen.service 
```

4. (Optionally) Add `logrotate` config file to make logs rotated:

```shell
sudo cp dnseen.logrotate /etc/logrotate.d/dnseen
```

Make sure config is valid:

```shell
sudo logrotate --debug /etc/logrotate.d/dnseen
```

If needed, force rotation and restart the service:

```shell
sudo logrotate --force /etc/logrotate.d/dnseen
sudo systemctl restart dnseen.service
```


## Use

Basic usage takes the whole log and prints the report without any
filters applied, domains ordered by number of hits in descending
order:

```shell
dnseen
```

when invoking command that is not under your `$PATH` (e.g. if you
followed the manual installation guide), use:

```shell
./dnseen
```

Apply some filters if needed:

```shell
dnseen \
    --from "2023-12-01T00:00:00" \
    --to "2024-01-01T00:00:00" \
    --match '\.(goog|google)$' \
    --exclude '(?i).*domains\.' \
    --hosts '/etc/hosts' \
    --hits 10 \
    --head 20 \
    --no-pretty \
    -vvv
```

A path to a file or a directory containing [hosts
file](https://man7.org/linux/man-pages/man5/hosts.5.html) can be
provided to get statistics about blocked domains, i.e. domains that
resolve to either [localhost](https://en.wikipedia.org/wiki/Localhost)
or [0.0.0.0](https://en.wikipedia.org/wiki/0.0.0.0). Use `--totals`
flag to get aggregation statistics of the report itself:

```shell
dnseen \
    --hosts '/etc/hosts.d/' \
    --hosts '/etc/hosts.old' \
    --totals
```

Find more options in help:

```shell
dnseen --help
```

Filters are applied to the raw logs in the order the corresponding CLI
options are shown in the help message (e.g. `--match` is applied
before `--exclude`).
