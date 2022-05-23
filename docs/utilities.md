# utilities

## Find a domains's DNS servers

### `host`

```
$ host -t ns somedomain.com
somedomain.com name server dns3.p05.nsone.net.
somedomain.com name server dns1.p05.nsone.net.
somedomain.com name server dns4.p05.nsone.net.
somedomain.com name server dns2.p05.nsone.net.
```

### `dig`
```
$ dig ns somedomain.com.com

; <<>> DiG 9.10.6 <<>> ns somedomain.com.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 51928
;; flags: qr rd ra; QUERY: 1, ANSWER: 4, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;somedomain.com.com.		IN	NS

;; ANSWER SECTION:
somedomain.com.com.	4264	IN	NS	dns3.p05.nsone.net.
somedomain.com.com.	4264	IN	NS	dns1.p05.nsone.net.
somedomain.com.com.	4264	IN	NS	dns4.p05.nsone.net.
somedomain.com.com.	4264	IN	NS	dns2.p05.nsone.net.

;; Query time: 11 msec
;; SERVER: 172.20.10.1#53(172.20.10.1)
;; WHEN: Sun Jul 11 13:02:33 CDT 2021
;; MSG SIZE  rcvd: 134
```

## Vimrc configuration

```
set number              " show line numbers
set wrap                " wrap lines
set encoding=utf-8      " set encoding to UTF-8 (default was "latin1")

"""" Tab settings

set tabstop=4           " width that a <TAB> character displays as
set expandtab           " convert <TAB> key-presses to spaces
set shiftwidth=4        " number of spaces to use for each step of (auto)indent
set softtabstop=4       " backspace after pressing <TAB> will remove up to this many spaces

set autoindent          " copy indent from current line when starting a new line
set smartindent         " even better autoindent (e.g. add indent after '{')
```

## Basics

### Open file

```
vim <file>
```

### Save file
```
# Write + quit
:wq
```

### Navigation

#### Top of File

```
gg
```

#### Bottom of File

```
GG
```

#### Left, Right, Up, Down (hjkl)
```
# left
h
# down
j
# up
k
# right
l
```