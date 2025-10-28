# Pre-Publication Security Checklist

Before publishing any new content to this repository:

## Required Checks

- [ ] Run secret scanner on all new files
- [ ] Verify no Task IDs are exposed (should be `[TASK-ID]`)
- [ ] Check for no internal URLs or IPs
- [ ] Remove any AWS/API keys
- [ ] Sanitize author internal email addresses
- [ ] Remove internal project code names
- [ ] Check figures for sensitive information
- [ ] Verify all citations are public
- [ ] Run pre-commit hook (automatic)
- [ ] Review diff before pushing

## Commands

```bash
# Scan specific file
python3 ~/Desktop/subvurs/github_secret_scanner.py

# Scan all staged files
git diff --cached --name-only | while read file; do
    echo "Scanning $file..."
    cat "$file" | python3 -c "from github_secret_scanner import SecretScanner; import sys; s = SecretScanner(); findings = s.scan_text(sys.stdin.read(), '$file'); print(f'Found {len(findings)} issues')"
done

# Check for internal markers
grep -r "INTERNAL" papers/
grep -r "Task ID:" papers/
grep -r -E "[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}" papers/
```

## Common Issues

1. **Task IDs**: Replace with `[TASK-ID]`
2. **Internal names**: Use sanitized versions
3. **IP addresses**: Remove or use `[IP-ADDRESS]`
4. **API keys**: Should never be in papers

## Approval Process

1. Local review with checklist
2. Pre-commit hook passes
3. GitHub Actions secret scan passes
4. Peer review (if applicable)
5. Publish to main branch
