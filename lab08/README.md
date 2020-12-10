## `lab08`

# Aluno
* Matheus Fernandes Lopes Silva
* RA: 222228

## Tarefas com Publicações

## Questão 1
Construa uma comando SELECT que retorne dados equivalentes a este XPath
~~~xquery
//individuo[idade>20]/@nome
~~~

### Resolução
~~~SQL
SELECT NOME
FROM INDIVIDUO
WHERE IDADE > 20;
~~~

## Questão 2
Qual a outra maneira de escrever esta query sem o where?

~~~xquery
let $fichariodoc := doc('https://raw.githubusercontent.com/santanche/lab2learn/master/xml/fichario.xml')
 
for $i in ($fichariodoc//individuo)
where $i[idade>17]
return {data($i/@nome)}
~~~
### Resolução
~~~xquery
let $fichariodoc := doc('https://raw.githubusercontent.com/santanche/lab2learn/master/xml/fichario.xml')
for $i in ($fichariodoc//individuo[idade>17])
return {data($i/@nome)}
~~~

## Questão 3
Escreva uma consulta SQL equivalente ao XQuery:
~~~xquery
let $fichariodoc := doc('https://raw.githubusercontent.com/santanche/lab2learn/master/xml/fichario.xml')

for $i in ($fichariodoc//individuo)
where $i[idade>17]
return {data($i/@nome)}
~~~

### Resolução
~~~sql
SELECT NOME
FROM INDIVIDUO
WHERE IDADE > 17;
~~~

## Questão 4
Retorne quantas publicações são posteriores ao ano de 2011.

### Resolução
~~~xquery
let $publicacoes := doc('https://raw.githubusercontent.com/santanche/lab2learn/master/data/publications/publications.xml')
for $i in ($publicacoes//publication[year>2011])
let $var := $i/publication
group by $var
return {'Posterior a 2011: ', count($i)}
~~~

## Questão 5
Retorne a categoria cujo `<label>` em inglês seja 'e-Science Domain'.

### Resolução
~~~xquery
let $publicacoes := doc('https://raw.githubusercontent.com/santanche/lab2learn/master/data/publications/publications.xml')
for $i in ($publicacoes//categories//category)
where $i[label='e-Science Domain']
return data($i/@key)
~~~

## Questão 6
Retorne as publicações associadas à categoria cujo `<label>` em inglês seja 'e-Science Domain'. A associação entre o label e a key da categoria deve ser feita na consulta.

### Resolução
~~~xquery
let $publicacoes := doc('https://raw.githubusercontent.com/santanche/lab2learn/master/data/publications/publications.xml')
for $i in ($publicacoes//categories//category[label='e-Science Domain']),
    $j in ($publicacoes/publications/publication)
where $j/key = $i/@key
return data($j)
~~~

## Tarefas com DRON e PubChem

## Questão 1

Liste o nome de todas as classificações que estão apenas dois níveis imediatamente abaixo da raiz.

### Resolução
~~~xquery
let $dron := doc('https://raw.githubusercontent.com/santanche/lab2learn/master/data/faers-2017-dron/dron.xml')
for $d in ($dron//drug//drug//drug)/@name
return {data($d), '&#xa;'}
~~~

## Questão 2

Apresente todas as classificações de um componente a sua escolha (diferente de `Acetylsalicylic Acid`) que estejam hierarquicamente dois níveis acima. Note que no exemplo dado em sala foi considerado um nível hierárquico acima.

### Resolução
~~~xquery
let $dron := doc('https://raw.githubusercontent.com/santanche/lab2learn/master/data/faers-2017-dron/dron.xml')
for $d in ($dron//drug[drug[drug/@name="METHENAMINE"]])
let $gr := $d/@name
group by $gr
return {data($gr), '&#xa;'}
~~~

## Questão 3

### Questão 3.1

Liste todos os códigos ChEBI dos componentes disponíveis.

#### Resolução
~~~xquery
(escreva aqui a resolução em XQuery)
~~~

### Questão 3.2

Liste todos os códigos ChEBI e primeiro nome (sinônimo) de cada um dos componentes disponíveis.

#### Resolução
~~~xquery
(escreva aqui a resolução em XQuery)
~~~

### Questão 3.3

Para cada código ChEBI, liste os sinônimos e o nome que aparece para o mesmo componente no DRON (se existir). Para isso faça um JOIN com o DRON.

#### Resolução
~~~xquery
(escreva aqui a resolução em XQuery)
~~~
