


## はじめに


## 事前準備[Linux(ubuntu)環境の構築]
### 1. dockerイメージをpullする

dockerコマンドを使ってLinux(ubuntu)環境の用意します。
```bash
docker pull ubuntu
```
:::details 実行結果を確認する
```bash
$ docker pull ubuntu
Using default tag: latest
latest: Pulling from library/ubuntu
Digest: sha256:77906da86b60585ce12215807090eb327e7386c8fafb5402369e421f44eff17e
Status: Image is up to date for ubuntu:latest
docker.io/library/ubuntu:latest
```
:::

### 2. dockerイメージが作成されているか確認する
```bash
docker images
```
:::details 実行結果を確認する
```bash
$ docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
ubuntu       latest    ca2b0f26964c   3 weeks ago   77.9MB
```
:::


### 3. 作成したdockerイメージでコンテナを起動する
```bash
docker run -d -it --name sample ubuntu
```
:::details 実行結果を確認する
```bash
$ docker run -d -it --name sample ubuntu
c4f422135b83f475dc2f2c44fcadeafce4047b6849e724ee672f160ddc8575cf
```
:::

### 4. 起動したコンテナの中に入る
```bash
docker exec -it sample bash
```
:::details 実行結果を確認する
```bash
$ docker exec -it sample bash
root@c4f422135b83:/#
```
:::

## 1. パッケージ管理(apt)
> “APT” は “Advanced Package Tool” の略で、DebianベースのLinuxディストリビューションで使用されるパッケージ管理システムです。 APTは、パッケージのインストール、アップグレード、削除、および依存関係の解決を自動化することができます。

- apt install [package]: パッケージをインストールします。
- apt remove [package]: パッケージを削除します。
- apt update: パッケージリストを更新します。
- apt upgrade: インストールされている全てパッケージを最新版にアップグレードします。
- apt install --only-upgrade [package]: 特定のパッケージを最新版にアップグレードします。

### 1. gitをaptでインストールする
apt updateでパッケージリストを最新化後、gitをインストールします。
```
apt update
apt install git
```
:::details 実行結果を確認する
```bash
root@c4f422135b83:/# apt update 
Get:1 http://security.ubuntu.com/ubuntu jammy-security InRelease [110 kB]
Get:2 http://archive.ubuntu.com/ubuntu jammy InRelease [270 kB]
Get:3 http://security.ubuntu.com/ubuntu jammy-security/main amd64 Packages [1619 kB]
Get:4 http://archive.ubuntu.com/ubuntu jammy-updates InRelease [119 kB]
Get:5 http://archive.ubuntu.com/ubuntu jammy-backports InRelease [109 kB]
Get:6 http://security.ubuntu.com/ubuntu jammy-security/multiverse amd64 Packages [44.6 kB]
Get:7 http://archive.ubuntu.com/ubuntu jammy/restricted amd64 Packages [164 kB]
Get:8 http://security.ubuntu.com/ubuntu jammy-security/universe amd64 Packages [1080 kB]
Get:9 http://archive.ubuntu.com/ubuntu jammy/multiverse amd64 Packages [266 kB]
Get:10 http://security.ubuntu.com/ubuntu jammy-security/restricted amd64 Packages [2037 kB]
Get:11 http://archive.ubuntu.com/ubuntu jammy/universe amd64 Packages [17.5 MB]
Get:12 http://archive.ubuntu.com/ubuntu jammy/main amd64 Packages [1792 kB]
Get:13 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 Packages [1898 kB]
Get:14 http://archive.ubuntu.com/ubuntu jammy-updates/universe amd64 Packages [1356 kB]
Get:15 http://archive.ubuntu.com/ubuntu jammy-updates/restricted amd64 Packages [2074 kB]
Get:16 http://archive.ubuntu.com/ubuntu jammy-updates/multiverse amd64 Packages [50.4 kB]
Get:17 http://archive.ubuntu.com/ubuntu jammy-backports/universe amd64 Packages [33.3 kB]
Selecting previously unselected package git.
Preparing to unpack .../36-git_1%3a2.34.1-1ubuntu1.10_amd64.deb ...
Unpacking git (1:2.34.1-1ubuntu1.10) ...
Selecting previously unselected package libldap-common.
Preparing to unpack .../37-libldap-common_2.5.17+dfsg-0ubuntu0.22.04.1_all.deb ...
Unpacking libldap-common (2.5.17+dfsg-0ubuntu0.22.04.1) ...
Selecting previously unselected package libsasl2-modules:amd64.
Preparing to unpack .../38-libsasl2-modules_2.1.27+dfsg2-3ubuntu1.2_amd64.deb ...
Unpacking libsasl2-modules:amd64 (2.1.27+dfsg2-3ubuntu1.2) ...
Selecting previously unselected package patch.
Preparing to unpack .../39-patch_2.7.6-7build2_amd64.deb ...
Unpacking patch (2.7.6-7build2) ...
Setting up libexpat1:amd64 (2.4.7-1ubuntu0.3) ...
Setting up libxau6:amd64 (1:1.0.9-1build5) ...
Setting up libpsl5:amd64 (0.21.0-1.2build2) ...
Setting up libcbor0.8:amd64 (0.8.0-2ubuntu1) ...
Setting up libbrotli1:amd64 (1.0.9-2build6) ...
Setting up libsasl2-modules:amd64 (2.1.27+dfsg2-3ubuntu1.2) ...
Setting up libnghttp2-14:amd64 (1.43.0-1ubuntu0.1) ...
Setting up less (590-1ubuntu0.22.04.2) ...
Setting up libx11-data (2:1.7.5-1ubuntu0.3) ...
Setting up librtmp1:amd64 (2.4+20151223.gitfa8646d.1-2build4) ...
Setting up patch (2.7.6-7build2) ...
Setting up libsasl2-2:amd64 (2.1.27+dfsg2-3ubuntu1.2) ...
Setting up libssh-4:amd64 (0.9.6-2ubuntu0.22.04.3) ...
Setting up libmd0:amd64 (1.0.4-1build1) ...
Setting up git-man (1:2.34.1-1ubuntu1.10) ...
Setting up netbase (6.3) ...
Setting up libfido2-1:amd64 (1.10.0-1) ...
Setting up openssl (3.0.2-0ubuntu1.15) ...
Setting up libbsd0:amd64 (0.11.5-1) ...
Setting up publicsuffix (20211207.1025-1) ...
Setting up libgdbm6:amd64 (1.23-1) ...
debconf: (No usable dialog-like program is installed, so the dialog based frontend cannot be used. at /usr/share/perl5/Debconf/FrontEnd/Dialog.pm line 78.)
debconf: falling back to frontend: Readline
Updating certificates in /etc/ssl/certs...
137 added, 0 removed; done.
Setting up libgdbm-compat4:amd64 (1.23-1) ...
Setting up libx11-6:amd64 (2:1.7.5-1ubuntu0.3) ...
Setting up libxmuu1:amd64 (2:1.1.3-3) ...
Setting up libperl5.34:amd64 (5.34.0-3ubuntu1.3) ...
Setting up openssh-client (1:8.9p1-3ubuntu0.6) ...
update-alternatives: using /usr/bin/ssh to provide /usr/bin/rsh (rsh) in auto mode
update-alternatives: warning: skip creation of /usr/share/man/man1/rsh.1.gz because associated file /usr/share/man/man1/ssh.1.gz (of link group rsh) doesn't exist
update-alternatives: using /usr/bin/slogin to provide /usr/bin/rlogin (rlogin) in auto mode
update-alternatives: warning: skip creation of /usr/share/man/man1/rlogin.1.gz because associated file /usr/share/man/man1/slogin.1.gz (of link group rlogin) doesn't exist
update-alternatives: using /usr/bin/scp to provide /usr/bin/rcp (rcp) in auto mode
update-alternatives: warning: skip creation of /usr/share/man/man1/rcp.1.gz because associated file /usr/share/man/man1/scp.1.gz (of link group rcp) doesn't exist
Setting up libxext6:amd64 (2:1.3.4-1build1) ...
Setting up libcurl3-gnutls:amd64 (7.81.0-1ubuntu1.15) ...
Setting up perl (5.34.0-3ubuntu1.3) ...
Setting up xauth (1:1.1-1build2) ...
Setting up liberror-perl (0.17029-1) ...
Setting up git (1:2.34.1-1ubuntu1.10) ...
Processing triggers for libc-bin (2.35-0ubuntu3.6) ...
Processing triggers for ca-certificates (20230311ubuntu0.22.04.1) ...
Updating certificates in /etc/ssl/certs...
0 added, 0 removed; done.
Running hooks in /etc/ca-certificates/update.d...
done.
```
:::

### 2. gitがインストールされている事を確認する
```bash
git version
```

:::details 実行結果を確認する。version 2.34.1がインストールされている事が確認出来ました。
```bash
root@c4f422135b83:/# git version
git version 2.34.1
root@c4f422135b83:/#
```
:::


## 2. 現在のディレクトリを表示する
現在は、ルートディレクトリにいると思うので、下記コマンドで確認します。
※/はルートディレクトリを表します。
```bash
pwd
```

:::details 実行結果を確認する。
```bash
root@c4f422135b83:/# pwd
/
root@c4f422135b83:/#
```
:::


## 3. ディレクトリ内のファイル/ディレクトリを表示する
```bash
ls
```

:::details 実行結果を確認する。
```bash
root@c4f422135b83:/# ls
bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@c4f422135b83:/#
```
:::


## 4. ディレクトリ内のファイル/ディレクトリの詳細を表示する

```bash
ll
```

:::details 実行結果を確認する。
```bash
root@c4f422135b83:/# ll
total 68
drwxr-xr-x   1 root root 4096 Mar 21 07:58 ./
drwxr-xr-x   1 root root 4096 Mar 21 07:58 ../
-rwxr-xr-x   1 root root    0 Mar 21 07:58 .dockerenv*
lrwxrwxrwx   1 root root    7 Feb 27 15:59 bin -> usr/bin/
drwxr-xr-x   2 root root 4096 Apr 18  2022 boot/
drwxr-xr-x   5 root root  360 Mar 21 07:58 dev/
drwxr-xr-x   1 root root 4096 Mar 21 08:08 etc/
drwxr-xr-x   2 root root 4096 Apr 18  2022 home/
lrwxrwxrwx   1 root root    7 Feb 27 15:59 lib -> usr/lib/
lrwxrwxrwx   1 root root    9 Feb 27 15:59 lib32 -> usr/lib32/
lrwxrwxrwx   1 root root    9 Feb 27 15:59 lib64 -> usr/lib64/
lrwxrwxrwx   1 root root   10 Feb 27 15:59 libx32 -> usr/libx32/
drwxr-xr-x   2 root root 4096 Feb 27 15:59 media/
drwxr-xr-x   2 root root 4096 Feb 27 15:59 mnt/
drwxr-xr-x   2 root root 4096 Feb 27 15:59 opt/
dr-xr-xr-x 233 root root    0 Mar 21 07:58 proc/
drwx------   2 root root 4096 Feb 27 16:02 root/
drwxr-xr-x   5 root root 4096 Feb 27 16:03 run/
lrwxrwxrwx   1 root root    8 Feb 27 15:59 sbin -> usr/sbin/
drwxr-xr-x   2 root root 4096 Feb 27 15:59 srv/
dr-xr-xr-x  11 root root    0 Mar 21 07:58 sys/
drwxrwxrwt   1 root root 4096 Mar 21 08:14 tmp/
drwxr-xr-x   1 root root 4096 Feb 27 15:59 usr/
drwxr-xr-x   1 root root 4096 Feb 27 16:02 var/
root@c4f422135b83:/# 
```
:::


## 5. ディレクトリ移動
:::message
目的のディレクトリへ移動
```bash
cd [移動先のディレクトリ名のパス]
```
1つ上のディレクトリへ移動
```bash
cd ..
```
前回のディレクトリに戻る
```bash
cd -
```
:::


## 6. ディレクトリの作成
:::message
```bash
mkdir [ディレクトリ名]
```
:::
./root配下にsampleという名前で作成します
```bash
cd ./root
mkdir sample
```

:::details 実行結果を確認する。
```bash
root@c4f422135b83:~# mkdir sample
root@c4f422135b83:~# ll
total 24
drwx------ 1 root root 4096 Mar 22 05:31 ./
drwxr-xr-x 1 root root 4096 Mar 21 07:58 ../
-rw------- 1 root root  109 Mar 21 23:54 .bash_history
-rw-r--r-- 1 root root 3106 Oct 15  2021 .bashrc
-rw-r--r-- 1 root root  161 Jul  9  2019 .profile
drwxr-xr-x 2 root root 4096 Mar 22 05:31 sample/
root@c4f422135b83:~#
```
:::


## 7. ファイルの作成
:::message
```bash
touch [ファイルのパス]
```
:::
sample/配下にmemo.mdを新規作成します。
```bash
touch memo.md
```

:::details 実行結果を確認する。
```bash
root@c4f422135b83:~/sample# touch memo.md
root@c4f422135b83:~/sample# ll
total 8
drwxr-xr-x 2 root root 4096 Mar 22 05:44 ./
drwx------ 1 root root 4096 Mar 22 05:31 ../
-rw-r--r-- 1 root root    0 Mar 22 05:44 memo.md
root@c4f422135b83:~/sample#
```
:::

## 8. ファイルの中身を表示

:::message
```bash
cat [ファイルのパス]
```
:::
.bashrc の中身を表示します
```bash
cat .bashrc 
```

:::details 実行結果を確認する。
```bash
root@c4f422135b83:~# cat .bashrc 
# ~/.bashrc: executed by bash(1) for non-login shells.
# see /usr/share/doc/bash/examples/startup-files (in the package bash-doc)
# for examples

# If not running interactively, don't do anything
[ -z "$PS1" ] && return

# don't put duplicate lines in the history. See bash(1) for more options  
# ... or force ignoredups and ignorespace
HISTCONTROL=ignoredups:ignorespace

# append to the history file, don't overwrite it
shopt -s histappend

# for setting history length see HISTSIZE and HISTFILESIZE in bash(1)
HISTSIZE=1000
HISTFILESIZE=2000

# check the window size after each command and, if necessary,
# update the values of LINES and COLUMNS.
shopt -s checkwinsize

# make less more friendly for non-text input files, see lesspipe(1)
[ -x /usr/bin/lesspipe ] && eval "$(SHELL=/bin/sh lesspipe)"

# set variable identifying the chroot you work in (used in the prompt below)
if [ -z "$debian_chroot" ] && [ -r /etc/debian_chroot ]; then
    debian_chroot=$(cat /etc/debian_chroot)
fi

# set a fancy prompt (non-color, unless we know we "want" color)
case "$TERM" in
    xterm-color) color_prompt=yes;;
esac

# uncomment for a colored prompt, if the terminal has the capability; turned
# off by default to not distract the user: the focus in a terminal window
# should be on the output of commands, not on the prompt
#force_color_prompt=yes

if [ -n "$force_color_prompt" ]; then
    if [ -x /usr/bin/tput ] && tput setaf 1 >&/dev/null; then
        # We have color support; assume it's compliant with Ecma-48
        # (ISO/IEC-6429). (Lack of such support is extremely rare, and such
        # a case would tend to support setf rather than setaf.)
        color_prompt=yes
    else
        color_prompt=
    fi
fi

if [ "$color_prompt" = yes ]; then
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
else
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
fi
unset color_prompt force_color_prompt

# If this is an xterm set the title to user@host:dir
case "$TERM" in
xterm*|rxvt*)
    PS1="\[\e]0;${debian_chroot:+($debian_chroot)}\u@\h: \w\a\]$PS1"
    ;;
*)
    ;;
esac

# enable color support of ls and also add handy aliases
if [ -x /usr/bin/dircolors ]; then
    test -r ~/.dircolors && eval "$(dircolors -b ~/.dircolors)" || eval "$(dircolors -b)"
    alias ls='ls --color=auto'
    #alias dir='dir --color=auto'
    #alias vdir='vdir --color=auto'

    alias grep='grep --color=auto'
    alias fgrep='fgrep --color=auto'
    alias egrep='egrep --color=auto'
fi

# some more ls aliases
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'

# Alias definitions.
# You may want to put all your additions into a separate file like
# ~/.bash_aliases, instead of adding them here directly.
# See /usr/share/doc/bash-doc/examples in the bash-doc package.

if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi

# enable programmable completion features (you don't need to enable
# this, if it's already enabled in /etc/bash.bashrc and /etc/profile
# sources /etc/bash.bashrc).
#if [ -f /etc/bash_completion ] && ! shopt -oq posix; then
#    . /etc/bash_completion
#fi
root@c4f422135b83:~#
```
:::

## 9. ファイル/ディレクトリの移動
:::message
```bash
mv [移動対象のパス] [移動先のパス]
```
:::
sample/配下にあるmemo.mdをsample/の外に移動します。
```bash
mv memo.md ../ 
```

:::details 実行結果を確認する。
```bash
root@c4f422135b83:~/sample# mv memo.md ../ 
root@c4f422135b83:~/sample# ll
total 8
drwxr-xr-x 2 root root 4096 Mar 22 05:55 ./
drwx------ 1 root root 4096 Mar 22 05:55 ../
root@c4f422135b83:~/sample# cd ..
root@c4f422135b83:~# ll
total 24
drwx------ 1 root root 4096 Mar 22 05:55 ./
drwxr-xr-x 1 root root 4096 Mar 21 07:58 ../
-rw------- 1 root root  109 Mar 21 23:54 .bash_history
-rw-r--r-- 1 root root 3106 Oct 15  2021 .bashrc
-rw-r--r-- 1 root root  161 Jul  9  2019 .profile
-rw-r--r-- 1 root root    0 Mar 22 05:44 memo.md
drwxr-xr-x 2 root root 4096 Mar 22 05:55 sample/
root@c4f422135b83:~#
```
:::
