# Aula Prática - 5 - Sistema de ficheiros

## Sistema de ficheiros

Vamos testar alguns comandos de mount e outros relativos ao sistema de ficheiros.

1. Crie um ficheiro para criar um sistema de ficheiros. Use:
dd if=/dev/zero of=disk bs=1024 count=10000
Veja o manual do dd para perceber os parâmetros.

        dd - convert and copy a file

        bs=BYTES        
            read and write up to BYTES bytes at a  time  (default: 512); overrides ibs and obs

        if=FILE
            read from FILE instead of stdin

        count=N
            copy only N input blocks


        $ dd if=/dev/zero of=disk bs=1024 count=10000
        10000+0 records in
        10000+0 records out
        10240000 bytes (10 MB, 9.8 MiB) copied, 0.0650225 s, 157 MB/s

2. Remova o ficheiro anterior e crie, usando o mesmo método, um de tamanho 20 MB

        https://stackoverflow.com/questions/139261/how-to-create-a-file-with-a-given-size-in-linux

        $ dd if=/dev/zero of=disk bs=1M count=20
        20+0 records in
        20+0 records out
        20971520 bytes (21 MB, 20 MiB) copied, 0.0569682 s, 368 MB/s


3. Formate o disco usando ext4:

        mkfs - build a Linux filesystem

        $ mkfs.ext4 disk
        mke2fs 1.45.5 (07-Jan-2020)
        Discarding device blocks: done                            
        Creating filesystem with 5120 4k blocks and 5120 inodes

        Allocating group tables: done                            
        Writing inode tables: done                            
        Creating journal (1024 blocks): done
        Writing superblocks and filesystem accounting information: done


4. Formate outra vez, mas usando xfs


        $ mkfs.xfs disk
        mkfs.xfs: disk appears to contain an existing filesystem (ext4).
        mkfs.xfs: Use the -f option to force overwrite.

5. Crie um diretório para onde montar o seu sistema e faça-o com:
mount disk "nome dir"

        $ sudo mount disk /Desktop/teste_AS/
        $ ls /Desktop/teste_AS/
        lost+found 
        

6. Use o diretório com o sistema (crie ficheiros) e depois desmonte-o.
        $ pwd
        /home/rui/Desktop/teste_AS
        $ sudo nano f1.txt
        $ sudo nano f2.txt

        umount - unmount file systems
        $ pwd 
        /home/rui/Desktop/teste_AS
        $ cd ..
        $ pwd 
        /home/rui/Desktop
        $ sudo umount ~/Desktop/teste_AS


7. Visualize o conteúdo do diretório. Crie ficheiros com o sistema desmontado.

        $ pwd
        /home/rui/Desktop
        $ ls ./teste_AS
        (vazio)
        $ sudo nano ./teste_AS/f3.txt
        $ ls ./teste_AS
        f3.txt

8. Monte novamente o sistema e veja o que aconteceu aos ficheiros. Desmonte e verifique novamente. O que pode concluir?

        $ pwd 
        /home/rui/Desktop
        $ sudo mount disk teste_AS/

        $ ls ./teste_AS
        f1.txt  f2.txt  lost+found  teste

        (Desapareceu ficheiro que criei apos umount)


        $ sudo umount ~/Desktop/teste_AS 
        $ ls ./teste_AS/
        f3.txt

        (Re-apareceu ficheiro que criei apos umount)


        Conclusao:
            -> Ao montar com mount um fylesystem, todos os ficheiros/diretorios
            que eu criar ficam la enquanto diretorio estiver montado
            -> Ao fazer umount os diretorios/ficheiros criados no mount ficam indesponiveis
            , e so sao guardado os diretorios/ficheiros criados no umount 
            -> Tudo isto no mesmo "disk"


## Utilização e permissões

1. Usar os comandos fuser, lsof e ps para ver quem está a utilizar ficheiros ou recursos de redes (ex: fuser 22/tcp ou lsof -i -a -p `pgrep -d ',' sshd`). Note que pode ter de fazer sudo para ter informação.

        $ sudo fuser 22/tcp
        (nao apareceu nada)

        $lsof -i -a -p 'pgrep -d'
        lsof: illegal process ID: pgrep -d 
        lsof 4.93.2
        latest revision: https://github.com/lsof-org/lsof
        latest FAQ: https://github.com/lsof-org/lsof/blob/master/00FAQ
        latest (non-formatted) man page: https://github.com/lsof-org/lsof/blob/master/Lsof.8
        usage: [-?abhKlnNoOPRtUvVX] [+|-c c] [+|-d s] [+D D] [+|-E] [+|-e s] [+|-f[gG]]
        [-F [f]] [-g [s]] [-i [i]] [+|-L [l]] [+m [m]] [+|-M] [-o [o]] [-p s]
        [+|-r [t]] [-s [p:s]] [-S [t]] [-T [t]] [-u s] [+|-w] [-x [fl]] [--] [names]
        Use the ``-h'' option to get more help information.


2. Use o chmod para exercitar as permissões. Nomeadamente crie um outro utilizador (ou use outro que já tenha criado) para testar o seu acesso. Teste o acesso de x aos diretórios (dê permissões de execução e depois tente com o utilizador entrar e listar os diretórios).


https://quickref.me/chmod.html

        rui$sudo chmod /home/rui/Desktop/Administracao_Sistemas 770 (retira todas as permissoes a others) 

        rui$sudo adduser teste 
        password: 1234

        (noutro terminal)
        rui$ su - teste
        password: 1234
        teste$cd ./Administracao_Sistemas/
        -bash: cd: ./Administracao_Sistemas/: Permission denied



3. Use agora setfacl e getfacl para os mesmos acessos. Com o utilizador criado anteriormente dê-lhe acessos que o grupo e os outros não têm.

        
        getfacl - get file access control lists
        rui$ getfacl ./Administracao_Sistemas/
        # file: Administracao_Sistemas/
        # owner: rui
        # group: rui
        user::rwx
        group::rwx
        other::---


https://www.geeksforgeeks.org/linux-setfacl-command-with-example/

        setfacl - set file access control lists

        setfacl -option file_owner:file_permission filename



