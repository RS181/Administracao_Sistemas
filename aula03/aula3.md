# Aula Prática - 3 - Processos

# Processos

1) Verifique quais os processos de kernel que estão a correr na máquina (nomes começam por k, e estão entre parênteses retos ex.: [kauditd]).

    ps -ef | grep  "\[k" 

2) Lance o browser várias vezes (pode fazê-lo da shell ou fazer Alt+F2 e indicar o comando). Mate os processos criados usando o killall (caso não esteja instalado instale o pacote correspondente; pode procurá-lo com dnf search killall).

    killall -e firefox

a) Experimente o kill no lxsession. Nota: se não existir um lxsession encontre um processo com session no nome. (deve-se ver o que são os processos antes de os matar). 

    // buscar um processo com session 
    $ps aux | grep session 
    // escolher um processo qualquer para matar 
    $kill [options] <pid> [...]

3) Veja a floresta de processos usando o ps e f (por exemplo ps -eF f)

    // tinha mais registos mas cortei senao eram muitos 
    $ ps -eF

    UID          PID    PPID  C STIME TTY          TIME CMD

    root           1       0  0 14:06 ?        00:00:02 /sbin/init splash
    
    root           2       0  0 14:06 ?        00:00:00 [kthreadd]
    
    root           3       2  0 14:06 ?        00:00:00 [rcu_gp]
    
    root           4       2  0 14:06 ?        00:00:00 [rcu_par_gp]
    
    .....

a) Use o pstree.
    
    pstree - display a tree of processes


4) Usando o -o ni (ver o manual para mostrar mais) para o ps, veja o estado de simpatia dos processos. 

    The -o flag allows you to specify columns. If you want to see your nice level, this would be in the NI column. So to see all processes with their nice level, do something like:

    $ ps ax -o pid,ni,cmd

a) Use o renice ou o nice para alterar o valor de um processo pouco simpático (com grande prioridade)

    renice [-n] priority [-g|-p|-u] identifier...

    $renice -n (valor mais alto) pid



# /Proc

1) Lance um processo (firefox por exemplo) ou escolha da lista do ps. Identifique o seu PID.

    $ libreoffice

    //obter o pid 
    $ ps aux | grep libreoffice

2) Veja no /proc/<PID> (substitua PID pelo que encontrou acima) os dados sobre esse processo

    $ cd /proc/<pid>
    $ ls 

3) Veja o conteúdo de /proc/mounts e compare com o comando mount.

    $ cat /proc/mounts > proc_mounts.txt
    
    //mount - mount a filesystem


    $ mount > mount.txt