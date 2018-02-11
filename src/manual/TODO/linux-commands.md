```bash
# search for directory
find / -type d -name 'httpdocs'

# grep
grep -E 'regular([0-9]{1,2})'

# get ip address
ifconfig wifi0 | grep -E '([1-9]{1}.([0-9]{1,3}.){3})([0-9]{1,3})' | cut -d: -f2 | awk '{print $1}'"
```