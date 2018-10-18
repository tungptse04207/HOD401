# Lab 1
- For single IP: nc -zv 35.240.173.172 1-3306
- For IP range: for i in {100..150}; do nc -zv 35.240.173.$i 1-3306; done

example output:

![](https://raw.githubusercontent.com/tungptse04207/HOD401/master/images/lab1.png)



# Lab 2
- Using knock to scan subdomain
- Installation guide:

git clone https://github.com/guelfoweb/knock.git

cd knock

chmod +x setup.py

sudo python setup.py install

- Running: knockpy google.com

Example output

![](https://raw.githubusercontent.com/tungptse04207/HOD401/master/images/lab2.png)



# Lab 3
- Idea: use ping command and wordlist from https://github.com/danielmiessler/SecLists/blob/master/Discovery/DNS/bitquark-subdomains-top100K.txt
- Usage: python3 scansubdomain.py [-d domain_name]
- Source code
```
import argparse
import subprocess
import re


def check_subdomain(target):
ps = subprocess.Popen(('ping', '-c', '1', '-t', '5', target),
stdout=subprocess.PIPE, stderr=subprocess.STDOUT)
output = ps.communicate()[0].decode('ASCII').split('\n')[0]
pattern = r'PING (.*?) \((.*?)\): .*'
try:
rs = re.match(pattern, output)
return [rs[2], rs[1]]
except:
return None


parser = argparse.ArgumentParser(prog='scansubdomain.py',add_help=False)
parser.add_argument('-h', action='store_true', dest='help')
parser.add_argument('-d', dest='domain')
try:
args = parser.parse_args()
if args.help or args.domain is None or len(args.domain) == 0:
print('Usage: scansubdomain.py [-d domain_name]')
elif args.domain is not None:
domain = str(args.domain).strip('http://')
with open('bitquark-subdomains-top100K.txt','rb') as f:
s = f.read().strip()
f.close()
for subdomain in s.split(b'\n'):
target = '{0}.{1}'.format(subdomain.decode('ASCII'), domain)
rs = check_subdomain(target)
if rs is not None:
print('{:<15}\t\t{}'.format(rs[0], target))

except:
pass

```

Example output<br>
![](https://raw.githubusercontent.com/tungptse04207/HOD401/master/images/lab3.png)

