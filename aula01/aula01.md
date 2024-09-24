# Contas do Sistema

## Exercicio 1

>man 5 passwd

/etc/passwd file is a text file that describes user login accounts for the system. It should
have read permission allowed for all users (many utilities, like ls(1) use it to map user  IDs  to
usernames), but write access only for the superuser.

Each line of the file describes a single user, and contains seven colon-separated fields(see documentation for more detailed explanation of each field):

    name:password:UID:GID:GECOS:directory:shell

    root:x:0:0:root:/root:/bin/bash
    fcup:x:1001:1001::/home/fcup:/bin/bash

    

>man 5 shadow

shadow ( shadowed password file )is a file which contains the password information for the system's accounts and optional aging information.

This file must not be readable by regular users if password security is to be maintained.

Each line of this file contains 9 fields, separated by colons (“:”), in the following order:

    name:encrypted_password:date_of_last_password change:minimum_password_age:maximum_password_age:password_warning_period:password_inactivity_period:account_expiration_date:reserved_field

FILES
    /etc/passwd
    User account information.

    /etc/shadow
    Secure user account information.


## Exercicio 2 

### a) 


Historically /etc/passwd had all of the user data, there was no shadow. However it was discovered that a dictionary attack could be done on the file, to discover passwords (if they are in the dictionary).

Therefore it was decided to remove the passwords from /etc/passwd, the rest of the file remained, as it was used by many programs, e.g. ls. The passwords were moved to /etc/shadow, and this file was made so that only root can read it.

    /etc/passwd now has an x for the password field.
    /etc/shadow only shares the first field (the key-field / the user name).
    /etc/shadow has been expanded to contain other password management fields.

## b) 

Justificacao na resposta anterior

## c)

Justificacao na aliena a)


# Exercicio 5)

## a)

cut -f 1,2,3,4,5,6,7 -d: /etc/passwd | sort

## b) 

cut -f 1 -d: /etc/passwd | uniq | wc -l

## c)

cut -f 7 -d: /etc/passwd | uniq | wc -l


> Nota: Segundo o professor uma conta sem password tem o caracter * no campo onde deveria estar a password
## d)

cut -f 7 -d: /etc/passwd |sort |uniq -c 

## e) 
grep -v  '[*]' shadow.txt | cut -f 1 -d:


## f)
grep  '[*]' shadow.txt | cut -f 1 -d:

# Procurar ficheiros

>Links Relevantes
>https://www.geeksforgeeks.org/find-command-in-linux-with-examples/

>find [path] [options] [expression]

## Exercicio 1
    md5sum - compute and check MD5 message digest

    https://linuxhandbook.com/find-exec-command/

find /home/ -maxdepth 2 -type f -exec md5sum {} \ > somas_md5_myHome;

## Exercicio 2 
    https://www.howtoforge.com/linux-md5sum-command/

md5sum --check somas_md5_myHome 

## Exercicio 3
    md5sum ./shadow.txt > md5sum_shadow
    (alterar shadow.txt)
    $ md5sum --check md5sum_shadow
    md5sum: WARNING: 1 computed checksum did NOT match  ./shadow.txt: FAILED

>Para que é que uma base de dados de sínteses criptográficas de ficheiros pode servir?

>R: Para garantir a integridade dos dados presentes nesses ficheiros

## Exercicio 4

find /  -executable -perm /111 -exec ls -l {} \;


## Exercicio 5

find / -executable -exec ls -l {} \; | grep -v -E 'total|root'

## Exercicio 6 
> nao usei o xargs
ls -t -l  /usr/bin/ | tail -n 1

## Exercicio 7

find / -executable -exec ls -t -l {} \; -quit  | grep root | head -n 1

## Exercicio 8
https://www.geeksforgeeks.org/environment-variables-in-linux-unix/

**Definicao de variaveis de ambiente**

    Environment variables, often referred to as ENVs, are dynamic values that wield significant influence over the behavior of programs and processes in the Linux operating system.

listar todos os ficheiros da sua home (o que tem a variável de ambiente $HOME)
    
    $cd /home    
    // ls -p tells ls to append a slash to entries which are a directory
    // grep -v / tells grep to return only lines not containing a slash
    $ls -p -a $HOME  /home/ | grep -v /

listar todos os ficheiros em /etc

    $ls -p /etc/ | grep -v /

https://ostechnix.com/find-files-based-permissions/

ficheiros debaixo /etc que tem permissoes de leitura
    
    $find /etc -erm -a=r

ficheiro mais recente debaixo de /etc que tenha permissões para ler.
    
    $find /etc -perm -a=r -exec ls  -p -t {} \;  -quit  | grep -v / | head -n 1

Juntando tudo

    $FILE=$(find /etc -perm -a=r -exec ls -p -t {} \; -quit | grep -v / | head -n 1)
    $LOCATION=$(readlink -f "$FILE")


> [!NOTE]
> Pedir ajuda ao professor para fazer este ultimo exercicio