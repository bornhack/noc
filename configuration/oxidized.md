# Automating configuration

Automating the network at BornHack is very helpful, especially gathering the configs during the event, in case a devices needs to be replaced.

Below are the configs used for Oxidized, https://github.com/ytti/oxidized


The config:
```
---
username: ...
password: ...
model: junos
interval: 600
use_syslog: false
log: "~/.config/oxidized/log"
debug: false
threads: 30
timeout: 20
retries: 3
prompt: !ruby/regexp /^([\w.@-]+[#>]\s?)$/
rest: 0.0.0.0:8888
next_adds_job: false
vars:
  enable: ""
groups: {}
models: {}
pid: "/home/oxidized/.config/oxidized/pid"
input:
  default: ssh, telnet
  debug: true
  ssh:
    secure: false
output:
  default: git
  git:
    user: Oxidized
    email: o@example.com
    repo: "/home/oxidized/gitrepo.git"

source:
  default: csv
  csv:
    file: "/home/oxidized/.config/oxidized/router.db"
    delimiter: !ruby/regexp /:/
    map:
      name: 0
      ip: 1
      model: 2
      username: 3
      password: 4
    gpg: false
model_map:
  cisco: ios
  juniper: junos
```

Sample router.db - add devices as appropriate:
```
born-core-01:10.123.44.10:junos:hlk:...
south1:10.123.44.15:ironware
south2:10.123.44.20:ironware
southwest1:10.123.44.21:ironware
south3:10.123.44.30:ironware
west1:10.123.44.40:ironware
west2:10.123.44.41:ironware
north1:10.123.44.42:ironware
```
