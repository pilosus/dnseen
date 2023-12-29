# Change Log

All notable changes to this project will be documented in this
file. This change log follows the conventions of
[keepachangelog.com](http://keepachangelog.com/).

## [Unreleased]

Nothing here yet.

### Added
- Installer script to automate installation process
  ([#1](https://github.com/pilosus/dnseen/issues/1))
- `--match` (or `-m`) option for a Perl-compatible regular expression to filter
  matching domain names
  ([#3](https://github.com/pilosus/dnseen/issues/3))
- `--verbose` (or `-v`) option added. Multiple flags combine into the
  total verbosity level
  ([#7](https://github.com/pilosus/dnseen/issues/7))

### Changed
- Query options section under the stats report is hidden by default,
  shown for verbosity level 1 and above

## [v0.1.0] - 2023-12-28
### Added
- `dnseen` analyzer with filtering by date range, domain exclude
  regex, number of hits and "take first N items"
- `systemd` service file
- `logrotate` config file

[Unreleased]: https://github.com/pilosus/dnseen/compare/v0.1.0...HEAD
[v0.1.0]: https://github.com/pilosus/dnseen/compare/v0.0.0...v0.1.0
