## Over-The-Wire -writeup-
### Bandit16 (bandit16@bandit.labs.overthewire.org)

---
#### Etape 1

Cette fois-ci il faut **chercher les ports ouverts entre 31000 et 32000**, si ils **communiquent en SSL**, et si c'est le cas, envoyer le mot de passe du niveau actuel.  
Pour cela j'utilise `nmap` qui va scanner les ports et trouver ceux qui sont ouverts:

```console
bandit16@bandit:~$ nmap localhost -p 31000-32000

Starting Nmap 7.40 ( https://nmap.org ) at 2019-12-17 01:44 CET
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00020s latency).
Not shown: 999 closed ports
PORT      STATE    SERVICE
31518/tcp filtered unknown
31790/tcp open     unknown

Nmap done: 1 IP address (1 host up) scanned in 1.25 seconds
```

L'option **-p** demande un intervalle dans lequel `nmap` va chercher.  
Ici on trouve qu'un port, le **31790**, est ouvert.

---
#### Etape 2

On se connecte donc sur le port en question:

```console
bandit16@bandit:~$ openssl s_client -connect localhost:31790
```

---
#### Etape 3

Puis on envoie le mot de passe du niveau actuel:

```console
cluFn7wTiGryunymYOu4RcffSxQluehd
Correct!
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----

closed
```

et on obtient une clé SSH privée.  
Il faut utiliser cette clé pour se connecter en tant que **bandit17**, sauvegardons donc cette clé dans un fichier, par exemple **priv_key**.

---
#### Etape 4

Avant toutes choses, si on essaie de se connecter avec `ssh -i priv_key bandit17@localhost`, il faut changer les permissions du fichier de la clé, avec `chmod 400 priv_key` tout simplement.

---
Désormais, on peut se connecter à **bandit17** sans problème

---
#### Etape 5

Finalement, on regarde le mot de passe de **bandit17** dans `/etc/bandit_pass/bandit17`, qui nous donne:
> xLYVMN9WE5zQ5vHacb0sZEVqbrp7nBTn

---
Mot de passe: **xLYVMN9WE5zQ5vHacb0sZEVqbrp7nBTn**

[<< bandit15.md](bandit15.md) | bandit16.md | [bandit17.md >>](bandit17.md)
