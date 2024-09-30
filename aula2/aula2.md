    Ter cuidado com o espacos dado ao fazer scripts de bash. De preferencia sempre que utilizar variaveis e quiser atribuir um valor fazer no formato:

+ var=( ... ) 

    Operacoes aritemticas tem de estar entre duplo parentesis

+ (( 1 + 2))
+ a=$(( $x + $y))

# Grupos 

## Exercicio 1

+ No /ect/group - ficheiro de grupo de utilizadores 
+ Cada entrada tem o formato o seguinte formato

    group_name:password:GID:user_list

    The fields are as follows:

    group_name  the name of the group.

    password    the (encrypted) group password.If this field is empty, no password is needed.

    GID         the numeric group ID.

    user_list   a list of the usernames that are members of  this group, separated by commas.



a=$(cut -f 3,4 -d: /etc/group | grep $username)

    Se usar "a=cut -f 3,4 -d: /etc/group | grep $username " imprime mas com a opcao <bold>a=$(...)</bold> isso faz com que nao haja impressao para o terminal

#faz um ciclo forEach pelas linhas do resultado obtido acima

    for i in $a
    do 
        group_id=$(echo $i | cut -f 1 -d:)
        group_name=$(getent group $group_id | cut -f 1 -d: )
        echo $group_name
    done


# Awk

## Exercicio 1

    #!/bin/bash

    a=$(du /etc/)

    soma=0

    for i in $a
    do 
        if echo $i | grep -q '^[0-9]\+$'; then
            # Soma o valor numérico à variável soma
            ((soma += i))
        fi
    done


    echo "soma total: $soma"


## Exercicio 2 
    #!/bin/bash

    a=$(du /etc/)
    nr_files=$(du /etc/ | wc -l )


    soma=0

    for i in $a
    do 
        if echo $i | grep -q '^[0-9]\+$'; then
            # Soma o valor numérico à variável soma
            ((soma += i))
        fi
    done

    #usamos o let para fazer um conta aritmetica e nao uma concatenacao de strings
    let media=$soma/$nr_files

    echo "Numero total ficheiros: $nr_files"
    echo "soma total: $soma"
    echo "media: $media"

    maior_ficheiro=$(du /etc/ | sort -g | awk '{print $1}'  | tail -n 1)
    menor_ficheiro=$(du /etc/ | sort -g | awk '{print $1}'  | head -n 1)


    echo "maior ficheiro: $maior_ficheiro"
    echo "maior ficheiro: $menor_ficheiro"

### alinea 

+ Epoc
    In a computing context, an epoch is the date and time relative to which a computer's clock and timestamp values are determined.
    The date and time in a computer are determined according to the number of seconds or clock ticks that have elapsed since the defined epoch for that computer or platform.

>#!/bin/bash
>
>
>soma=0
>count=0
>maximo=0
>minimo=0
>
>#Executa o comando ls --full-time (recebe com input) e lê o resultado linha a 
>#linha
>while read -r linha; 
>do
>    #echo $linha
>
>    epoch=$(date -d "$linha" +%s)
>    
>    #Atualiza o mínimo e o máximo
>    if [[ $count -eq 0 ]]; then
>        # Para o primeiro valor, inicializa mínimo e máximo
>        minimo=$epoch
>        maximo=$epoch
>    else
>        if [[ $epoch -lt $minimo ]]; then
>            minimo=$epoch
>        fi
>        if [[ $epoch -gt $maximo ]]; then
>            maximo=$epoch
>        fi
>    fi
>    
>    ((count++))
>    soma=$(( soma + epoch))
>
>done < <(ls --full-time /etc | awk '{print $6,$7}')
>
>echo "soma total : "$soma
>echo "numero linhas : "$count
>
>let media=$soma/$count
>echo "media: "$media
>echo "mínimo Epoch: $minimo"
>echo "máximo Epoch: $maximo"
