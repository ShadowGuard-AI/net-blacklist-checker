![Banner](banner.png)

# net-blacklist-checker
Checks an IP address or domain against public DNS blacklists by DNS query, and prints a listed or not-listed line for each list checked. Focused on Basic network operations.

## Install
```bash
git clone https://github.com/ShadowGuard-AI/net-blacklist-checker.git
cd net-blacklist-checker
pip install -r requirements.txt
```

Python 3 plus the `requests` package, which is imported at startup (see `requirements.txt`). The checks themselves are DNS queries made with the standard library, not HTTP calls.

## Usage
```bash
python3 main.py -i 203.0.113.10
python3 main.py -d example.com -l spamhaus
```

Exactly one of `-i` and `-d` is required.

## Parameters
- `-i`, `--ip`: The IP address to check. IPv4 only.
- `-d`, `--domain`: The domain to check. The domain must resolve in DNS at validation time, or it is rejected as invalid.
- `-l`, `--list`: Specify a particular list instead of all blacklists. Choices: `spamhaus`, `abuseipdb`, `bl_spamcop`, `psbl`. Default: all four.
- `-h`, `--help`: Show help message and exit.

Lists checked, as defined in code: `spamhaus` (zen.spamhaus.org), `abuseipdb` (check.abuseipdb.com), `bl_spamcop` (bl.spamcop.net), `psbl` (psbl.surriel.com).

## Limits, as the code states them
- Any DNS answer at all counts as listed; the returned address is never read, so listing codes are not distinguished, and a zone that answers for everything looks like a hit.
- A DNS name error reads as not listed, so a DNS outage is indistinguishable from a clean result.
- The `abuseipdb` entry names a web API host, not a DNS blacklist zone, so its DNS query never gets a real answer from that service.
- The `spamhaus` entry is typed as a domain query in code, not as a reversed-IP query.

## Exit codes
- `0`: the checks ran, whether listed or not. The verdict is in the printed lines only; the exit code never carries it.
- `1`: invalid IP address, invalid domain, or no target.
- `2`: missing required argument (argparse).

## License
MIT. See [LICENSE](LICENSE).
