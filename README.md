# Wordlist Collection

A curated collection of password wordlists, username lists, and default credentials for **authorized security testing and penetration testing**.

> ⚠️ **Legal Disclaimer**: These wordlists are provided strictly for security research, authorized penetration testing, and educational purposes. Unauthorized access to computer systems is illegal. Always obtain proper authorization before testing any system.

## 📁 Directory Structure

```
wordlist/
├── README.md
├── LICENSE
├── .gitignore
│
├── passwords/              # Password wordlists (one password per line)
│   ├── common.txt           # General weak passwords (1.3M+ entries)
│   ├── common_small.txt     # Top 1000 most common weak passwords
│   ├── web.txt              # Web application common passwords
│   ├── ssh.txt              # SSH honeypot collected passwords (cleaned)
│   ├── rdp.txt              # RDP common passwords
│   ├── ftp.txt              # FTP default/common passwords
│   ├── databases.txt        # Database default passwords (MySQL, PostgreSQL, Redis, etc.)
│   ├── iot.txt              # IoT/smart home/industrial default passwords
│   └── chinese_weak.txt     # Chinese-context weak passwords (pinyin, lucky numbers)
│
├── usernames/               # Username lists
│   ├── common.txt           # General username list (81K+ entries)
│   └── admin.txt            # Admin/common service account usernames
│
├── defaults/                # Default credentials (structured JSON)
│   ├── ip_cameras.json      # IP camera/NVR default credentials
│   ├── databases.json       # Database default credentials with ports
│   ├── nas.json             # NAS device default credentials
│   └── iot.json             # IoT/router/networking default credentials
│
├── leaked/                  # Passwords from public data breaches
│   ├── adobe_top100.txt     # Top 100 from Adobe 2013 breach
│   └── README.md            # Data source descriptions
│
└── tools/                   # Utility scripts
    ├── clean.py             # Wordlist cleaner (remove junk, dedupe, normalize)
    ├── mangler.py           # Password variant generator (leet, case, suffixes)
    ├── generate_chinese.py  # Chinese weak password generator
    ├── merge_dedupe.sh      # Merge and deduplicate wordlists
    └── wordlist_stats.py    # Analyze and report wordlist statistics
```

## 🔧 Tools

### clean.py - Wordlist Cleaner

Clean and validate password wordlists by removing junk data.

```bash
# Basic cleaning
python3 tools/clean.py -i dirty.txt -o clean.txt

# With length filter and control character removal
python3 tools/clean.py -i dirty.txt -o clean.txt --min-len 4 --max-len 64 --no-control

# Specify encoding
python3 tools/clean.py -i dirty.txt -o clean.txt --encoding latin-1

# Dry run (stats only)
python3 tools/clean.py -i dirty.txt -o /dev/null --dry-run
```

### mangler.py - Password Variant Generator

Generate password variants from a base wordlist.

```bash
# Apply case + leet speak rules
python3 tools/mangler.py -i passwords/common_small.txt -o mangled.txt --rules case,leet

# Apply all rules with length limit
python3 tools/mangler.py -i passwords/common_small.txt -o mangled.txt --rules all --max-len 16

# Limit output size
python3 tools/mangler.py -i passwords/common_small.txt -o mangled.txt --rules all --limit 100000
```

Available rules: `case`, `leet`, `numbers`, `special`, `years`, `reverse`, `prefix`, `all`

### generate_chinese.py - Chinese Password Generator

Generate Chinese-context weak passwords (pinyin, lucky numbers, names).

```bash
# Generate to default path
python3 tools/generate_chinese.py -o passwords/chinese_weak_generated.txt

# Limit output size
python3 tools/generate_chinese.py -o passwords/chinese_weak_generated.txt --limit 50000
```

### merge_dedupe.sh - Merge & Deduplicate

Merge multiple wordlists into one sorted, deduplicated file.

```bash
# Merge multiple files
bash tools/merge_dedupe.sh -o merged.txt passwords/ssh.txt passwords/web.txt

# With length filter
bash tools/merge_dedupe.sh -o merged.txt --min-len 6 --max-len 32 passwords/*.txt
```

### wordlist_stats.py - Wordlist Analyzer

Analyze wordlist files and show statistics.

```bash
# Analyze specific files
python3 tools/wordlist_stats.py passwords/ssh.txt passwords/web.txt

# Analyze all wordlists
python3 tools/wordlist_stats.py --all

# JSON output
python3 tools/wordlist_stats.py passwords/*.txt --json
```

## 📊 Wordlist Statistics

| File | Entries | Description |
|------|---------|-------------|
| passwords/common.txt | 1.3M+ | General weak passwords (comprehensive) |
| passwords/common_small.txt | 1,000 | Top 1000 most common |
| passwords/ssh.txt | 79K+ | SSH honeypot collected (cleaned) |
| passwords/rdp.txt | 209K+ | RDP common passwords |
| passwords/web.txt | 111 | Web application passwords |
| passwords/ftp.txt | 38 | FTP default passwords |
| passwords/databases.txt | 49 | Database default passwords |
| passwords/iot.txt | 81 | IoT/smart device passwords |
| passwords/chinese_weak.txt | 162 | Chinese-context weak passwords |
| usernames/common.txt | 81K+ | General usernames |
| usernames/admin.txt | 71 | Admin account names |

## 🔄 Workflow

### Quick Start
```bash
# 1. Use a pre-built wordlist directly
hydra -l admin -P passwords/web.txt target http-post-form "/login:user=^USER^&pass=^PASS^"

# 2. Generate custom variants
python3 tools/mangler.py -i passwords/common_small.txt -o my_custom.txt --rules case,leet,numbers

# 3. Merge multiple sources
bash tools/merge_dedupe.sh -o custom.txt --min-len 4 passwords/web.txt passwords/ftp.txt passwords/databases.txt

# 4. Check stats
python3 tools/wordlist_stats.py custom.txt
```

### Adding New Wordlists
1. Place new `.txt` files in the appropriate directory (`passwords/`, `usernames/`)
2. Run `python3 tools/clean.py -i new_file.txt -o new_file.txt` to normalize
3. Update this README with the new entry

## 📝 Data Sources

- **ssh_passwd.txt**: Collected from SSH honeypots (2015), cleaned and deduplicated
- **common.txt**: Curated from multiple public sources (rockyou derivatives)
- **rdp_passlist.txt**: RDP-specific weak passwords
- **adobe_top100.txt**: Top 100 from the Adobe 2013 data breach (130M accounts)
- **router_default_password.md**: Originally sourced from portforward.com
- **Default credentials**: Manually curated from vendor documentation

## 🤝 Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Add new wordlists in the appropriate directory
3. Run `python3 tools/clean.py` to normalize your additions
4. Run `python3 tools/wordlist_stats.py --all` to verify quality
5. Update this README
6. Submit a pull request

## 📜 License

MIT License - see [LICENSE](LICENSE) for details.

## ⚖️ Responsible Use

This toolkit is designed for:
- ✅ Authorized penetration testing
- ✅ Security research and education
- ✅ Password policy auditing
- ✅ CTF competitions and training
- ❌ Unauthorized system access
- ❌ Any illegal activity
