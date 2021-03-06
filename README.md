# Systemd user resource limitation imposer

**resrc** (i.e. Resource Control) imposes arbitrary CGroup resource constraints upon every user
session when they are first created.  In other words, when a user
first logs in, **resrc** receives a signal and invoke Systemd's API to set
constraint over her resource usage.

See [here](https://www.freedesktop.org/software/systemd/man/systemd.resource-control.html)
for a list of available resource type.

See `examples/config.yaml` for example configuration file.

### Install

```
python setup.py install
```

### Develop

```
# Work in a virtual environment
virtualenv env

# Jump into it.
source env/bin/activate

# In another terminal window, automatically re-install the scripts when they are being modified
\ls bin/* lib/*.py | entr -s 'python setup.py develop' 
```

### Package

#### ArchLinux

```
makepkg
```

### Example (rules read from commandline)

Apply the `MemoryMax` Systemd resource rule to the user `john`:

```
$ resrctl --users john --rules MemoryMax=53687091200 # 50 GiB in bytes
```

Apply the `MemoryMax` (in percentage) resource limit to users `alice` and `bob:

```
$ resrctl --users alice,bob --rules MemoryMax=40% # 40% of RAM
```

Apply the `MemoryMax` (in percentage) resource limit to users `alice` and `bob`:

```
$ resrctl --users alice,bob --rules MemoryMax=40% # 40% of RAM
```

Apply the `CPUQuota` (in percentage) resource limit to users `doge` and `meow`
and all users in group `faculty` and `student`:

```
$ resrctl --users doge,meow --groups faculty,student --rules CPUQuota=40% # 0.4 vCPU
```

Apply the `CPUQuotaOverall` (in percentage) resource limit to user `doge`:

```
# If the system has 8 vCPU, doge will get 8*0.2 = 1.6 vCPU
$ resrctl --users doge --rules CPUQuotaOverall=20%
```

### Example (rules read from configuration file)
```
$ resrctl --config examples/config.yaml
```
