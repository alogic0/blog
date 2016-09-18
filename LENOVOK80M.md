Problem on Lenovo K80M
```
root@localhost:~# ghc-8.0.1-bin/bin/ghc -V
ghc: failed to create OS thread: Cannot allocate memory
root@localhost:~# ulimit -a
core file size          (blocks, -c) 0
data seg size           (kbytes, -d) unlimited
scheduling priority             (-e) 40
file size               (blocks, -f) unlimited
pending signals                 (-i) 31107
max locked memory       (kbytes, -l) 64
max memory size         (kbytes, -m) unlimited
open files                      (-n) 1024
pipe size            (512 bytes, -p) 8
POSIX message queues     (bytes, -q) 819200
real-time priority              (-r) 0
stack size              (kbytes, -s) 2359296
cpu time               (seconds, -t) unlimited
max user processes              (-u) 31107
virtual memory          (kbytes, -v) unlimited
file locks                      (-x) unlimited
root@localhost:~# ulimit -s 1000000
root@localhost:~# ghc-8.0.1-bin/bin/ghc -V
ghc: failed to create OS thread: Cannot allocate memory
```

The solution:
```
root@localhost:~# ulimit -s 820000
root@localhost:~# ghc-8.0.1-bin/bin/ghc -V
The Glorious Glasgow Haskell Compilation System, version 8.0.1
```

```
root@localhost:~/ghc-8.0.1# time make install
real    70m11.576s
user    3m21.500s
sys     19m1.560s
```

Another hack for `git`:
```
git config --global pack.threads "1"
```

bootstraping `cabal-install` requires `zlib1g-dev` C-library.

After building and installing `cabal-install` to run `cabal` it's necessary to downgrade stack size again!
And for building `old-time` package `ulimit` has to be lowered too! At the end I had:
```
ulimit -s 300000
```
I'm not sure that this limit is final.

Building `cabal-install`:
```
root@localhost:~/src/cabal-install-1.24.0.0# time ./bootstrap.sh

real    88m52.952s
user    34m51.790s
sys     16m37.210s
```

Building `hakyll-4.8.3.2`
```
root@localhost:~# time cabal install hakyll

real    74m11.190s
user    91m1.240s
sys     10m28.860s
```
