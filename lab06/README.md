## `lab06`

# Aluno
* Matheus Fernandes Lopes Silva
* RA: 222228

## Tarefa de Cypher e o FDA Adverse Event Reporting System (FAERS)

## Exercício 1

Escreva uma sentença em Cypher que crie o medicamento de nome `Metamizole`, código no DrugBank `DB04817`.

### Resolução
~~~cypher
CREATE (:Drug {drugbank: "DB04817", name:"Metamizole"})
~~~

## Exercício 2

Considerando que a `Dipyrone` e `Metamizole` são o mesmo medicamento com nomes diferentes, crie uma aresta com o rótulo `:SameAs` que ligue os dois.

### Resolução
~~~cypher
CREATE (a:Drug {name:"Dipyrone"});

MATCH (a:Drug {name:"Dipyrone"})
MATCH (b:Drug {name:"Metamizole"})
CREATE ( (a)-[:SameAs]-> (b))
~~~

## Exercício 3

Use o `DELETE` para excluir o relacionamento que você criou (apenas ele).

### Resolução
~~~cypher
MATCH ((a)-[n:SameAs]->(b))
DELETE (n)
~~~

## Exercício 4

Faça a projeção em relação a Patologia, ou seja, conecte patologias que são tratadas pela mesma droga.

### Resolução
~~~cypher
MATCH (p1:Pathology)-[a]-(d:Drug)-[b]-(p2:Pathology)
MERGE (p1)<-[r:PatRelates]->(p2)
ON CREATE SET r.weight=1
ON MATCH SET r.weight=r.weight+1
~~~

## Exercício 5

Construa um grafo ligando os medicamentos aos efeitos colaterais (com pesos associados) a partir dos registros das pessoas, ou seja, se uma pessoa usa um medicamento e ela teve um efeito colateral, o medicamento deve ser ligado ao efeito colateral.

### Resolução
~~~cypher
LOAD CSV WITH HEADERS FROM 'https://raw.githubusercontent.com/santanche/lab2learn/master/data/faers-2017/drug-use.csv' AS line
CREATE (:UseDrug {idperson: line.idperson, codepathology: line.codepathology, codedrug: line.codedrug});

LOAD CSV WITH HEADERS FROM 'https://raw.githubusercontent.com/santanche/lab2learn/master/data/faers-2017/sideeffect.csv' AS line
CREATE (:SideEffect {idPerson: line.idPerson, codePathology: line.codePathology});

MATCH (d:Drug)
MATCH (u:UseDrug)
MATCH (s:SideEffect)
WHERE u.idperson = s.idPerson AND d.code = u.codedrug
MERGE (d)-[r:SideDrug]->(s)
ON CREATE SET r.weight=1
ON MATCH SET r.weight=r.weight+1;
~~~

## Exercício 6

Que tipo de análise interessante pode ser feita com esse grafo?

Proponha um tipo de análise e escreva uma sentença em Cypher que realize a análise.

### Resolução
Projeção para ligar medicamentos que possuem o mesmo efeito colateral.
~~~cypher
MATCH (u:UseDrug)-[a]->(s:SideEffect)<-[b]-(u:UseDrug)
MERGE (u)<-[r:DrugRelate]->(s)
ON CREATE SET r.weight=1
ON MATCH SET r.weight=r.weight+1
~~~
