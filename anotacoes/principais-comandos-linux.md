# Principais comandos Linux

---

`cd`: change directory - entra e sai de diretórios

## ls
    comando que lista arquivos e diretórios.

    `ls -ltr`: lista do mais antigo para o mais recente

    `ls -l`: exibe detalhes dos arquivos e diretórios

    `ls -1`: lista por linha sem os detalhes

## ps

Exibe processos do usuário na sessão atual.

`ps axu`: exibe todos os processos de todos os usuários e demais detalhes

## touch

Pode ser utilizado para criar um arquivo em branco e atualizar o horário de arquivos

## echo

exibe qualquer parâmetro passado para ele

`echo -n` exibe sem quebra de linha

`echo -e` habilita o aceite de parâmetros especiais (semelhante ao print format de algumas linguagens)

## mkdir

Comando para criação de diretórios.

`mkdir -p`: opção que permite a criação de toda a árvore de diretórios (quando o primeiro diretório não existe)

## rm

remove arquivos e diretórios (quando utilizando a flag -r)

`rm -r`: remove um diretório recursivamente.

`rm -f`: apaga um arquivo forçadamente.

`rm -rf`: remove diretórios recursivamente e forçadamente.

## sleep

O comando sleep ativa o modo de espera do shell. O comando é definido por `sleep <segundos>`.

## cat

O comando `cat` exibe o conteúdo de um arquivo. Sua sintaxe básica é `cat <arquivo>`.

`cat -b`: exibe o conteúdo enumerando as linhas que não estão em branco.
`-n`: enumera as linhas, independente se estão em branco ou não.
`-A`: exibe caracteres especiais.

## tac

Semelhante ao `cat`, mas exibe o conteúdo em ordem inversa.

## tail

Exibe as últimas linhas de um arquivo. Por padrão são as últimas 10 linhas.

`-n`: permite definir a quantidade de linhas. O `-n` pode ser omitido utilizando apenas o `-` seguido do número que define a quantidade.

## head

Exibe as primeiras linhas de um arquivo.

`-n` controla a quantidade de linhas a serem exibidas.
`-c` controla o número de caracteres a serem exibidos.

## wc

Exibe a quantidade de linhas, palavras e caracteres de um arquivo.

`-l` exibe o número de linhas apenas
`-w` exibe o número de palavras
`-m` exibe o número de caracteres
`-c` exibe o número de bytes

É possível utilizar o comando combinado com o coringa `*` para poder contar em mais de um arquivo. Por exemplo:

~~~bash
wc alunos*
~~~

~~~
13 24 191 alunos2.txt
9 19 77 alunos3.txt
10 10 70 alunos.txt
32 44 338 total
~~~

## Utilizando pipe (|) para direcionar a saída de um comando para a entrada de outro

Isso nos permite combinar mais de um comando. Por exemplo:

~~~bash
ls | grep nome
~~~

Lista apenas os arquivos que contém o nome 'nome'.

~~~bash
tail -n5 alunos2.txt | wc -w
~~~

Exibe a quantidade de palavras das 5 últimas linhas do documento 'alunos2.txt'.

## sort

Ordena o conteúdo de um arquivo.

`-r` ordena de forma inversa.
`-k[indice]` ordena a partir do índice (`sort -k2` ordena a partir do segundo campo).
`-t"[delimitador]"` auxília a ordenação identificando qual o limitador para o campo que está sendo utilizado como índice. O delimitador é algum conteúdo presente no conteúdo exibido.

Exemplo:

~~~bash
tail /etc/passwd | sort -k3 -t":"
~~~

Resultado:

~~~
thiago:x:1000:1000:Thiago,,,:/home/thiago:/usr/bin/zsh
avahi:x:118:126:Avahi mDNS daemon,,,:/var/run/avahi-daemon:/usr/sbin/nologin
saned:x:119:127::/var/lib/saned:/usr/sbin/nologin
lightdm:x:120:128:Light Display Manager:/var/lib/lightdm:/bin/false
colord:x:121:130:colord colour management daemon,,,:/var/lib/colord:/usr/sbin/nologin
pulse:x:122:131:PulseAudio daemon,,,:/var/rp
~~~

Porém, acima percebe-se que o conteúdo foi ordenado como string, apesar do índice escolhido ter sido o terceiro campo que contém apenas números. Para que seja ordenado como valores numéricos, utiliza a opção `-g` ao fim do comando:

~~~bash
tail /etc/passwd | sort -k3 -t":" -g
~~~

## uniq

Exibe os conteúdos não repetidos, porém, por padrão só considera repetições sequenciais. Para resolver, podemos utilizar o `uniq` combinado com o comando `sort`:

~~~bash
sort alunos.txt | uniq
~~~

~~~
José
Josefa
Manu
Maria
Ricardo
Thiago
~~~

O mesmo arquivo apenas com o uso do comando `cat` para que possamos ver como está organizado originalmente e entender o comportamento do comando `uniq`.

~~~bash
cat alunos.txt
~~~

~~~
Thiago
João
Ricardo
Maria
José
Josefa
Maria
José
Thiago
Ricardo
Manu
Manu
~~~

Agora utilizando apenas o comando `uniq` para observarmos o resultado:

~~~bash
uniq alunos.txt
~~~

~~~
Thiago
João
Ricardo
Maria
José
Josefa
Maria
José
Thiago
Ricardo
Manu
~~~

> Perceba que apesar de ainda haver repetições, elas não foram consideradas em razão do seu padrão de só considerar as repetições em sequência. Por isso é importante a combinação com o comando `sort`.

Por fim, para exibir apenas os valores que não se repetem, utilizamos a opção `-u`:

~~~bash
sort alunos.txt | uniq -u
~~~

Resultado:

~~~
João
Josefa
~~~

A opção `-d` fará o serviço oposto, exibirá apenas as linhas que se repetem:

~~~bash
sort alunos.txt | uniq -d
~~~

Resultado:

~~~
José
Manu
Maria
Ricardo
Thiago
~~~

A opção `-c` conta as repetições:

~~~bash
sort alunos.txt | uniq -c
~~~

Resultado:

~~~
1 João
2 José
1 Josefa
2 Manu
2 Maria
2 Ricardo
2 Thiago
~~~

De forma bastante lógica, é possível combinar ainda mais os comandos uniq e sort fazendo o uso de pipe, por exemplo, para ordenar a saída acima na ordem do que menos se repete para o que mais se repete:

~~~bash
sort alunos.txt | uniq -c | sort
~~~

~~~
1 João
1 Josefa
2 José
2 Manu
2 Maria
2 Ricardo
2 Thiago
~~~
