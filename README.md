# Explorando Práticas de Teste

Neste exercício, vamos explorar práticas de teste em sistemas reais utilizando a ferramenta [TestMiner](https://andrehora.github.io/testminer).

O TestMiner permite visualizar e analisar testes de software em repositórios do GitHub, fornecendo dados sobre como os projetos organizam seus testes, como eles evoluem entre versões e quais bibliotecas de teste são utilizadas.
Explore a ferramenta antes de começar para se familiarizar com seu funcionamento.

---

## Passo 1: Selecionar um repositório

Escolha um repositório real que possua testes escritos na linguagem de sua preferência.
Abaixo estão alguns links para ajudá-lo a encontrar projetos interessantes:

- **Python:** https://github.com/topics/python?l=python
- **JavaScript:** https://github.com/topics/javascript?l=javascript
- **TypeScript:** https://github.com/topics/typescript?l=typescript
- **Java:** https://github.com/topics/java?l=java

## Passo 2: Explorar o repositório selecionado

Busque o repositório escolhido no [TestMiner](https://andrehora.github.io/testminer) e analise os dados de teste gerados pela ferramenta.

## Passo 3: Explicar uma prática de teste

Com base nos dados obtidos, selecione uma prática ou dado de teste relevante e explique-o com suas próprias palavras.

---

## Instruções de entrega

1. Faça um `fork` deste repositório (saiba mais sobre forks [aqui](https://docs.github.com/pt/pull-requests/collaborating-with-pull-requests/working-with-forks/fork-a-repo)).
2. Responda às questões abaixo diretamente neste arquivo `README.md` do seu fork. Pode adicionar imagens para enriquecer sua explicação.
3. No Moodle, submeta apenas a URL do seu fork.

---

## Respostas

**1. Repositório selecionado:** [scikit-learn](https://github.com/scikit-learn/scikit-learn)

**2. Explicação:** O scikit-learn é um framework que provê a implementação de diversos algoritmos de aprendizado de máquina
além de ferramentas estatísticas, ou até mesmo *toy datasets*. Em particular, me chamou a atenção o uso de mocks com o objetivo
de *mockar* classificadores. E existe uma bateria de testes que testam o funcionamento desse mock. Basicamente, o objetivo do
mock é prover uma forma de testar pipelines de machine learning, i.e., uma sequência de processamento nos dados seguida de um
treinamento ou classificação com algum dos algoritmos. O mock não só funciona como um mock para classificadores, mas também
consegue verificar diversas propriedades do input inicial (X,y) ao longo do pipeline completo, de acordo com alguma função 
particular de verificação. A classe de mock pode ser encontrada [aqui](https://github.com/scikit-learn/scikit-learn/blob/main/sklearn/utils/_mocking.py). Um exemplo pode ser visto abaixo:

>>> from sklearn.utils._mocking import CheckingClassifier
>>> This helper allow to assert to specificities regarding 'X' or 'y'. In this`
>>> case we expect 'check_X' or 'check_y' to return a boolean.`
>>> from sklearn.datasets import load_iris
>>> X, y = load_iris(return_X_y=True)
>>> clf = CheckingClassifier(check_X=lambda x: x.shape == (150, 4))
>>> clf.fit(X, y)
>>> CheckingClassifier(...)

Para testar essa classe, foi feito uma [classe de testes para o mock](https://github.com/scikit-learn/scikit-learn/blob/main/sklearn/utils/tests/test_mocking.py), que faz o uso de fixtures para inicializar os datasets
e para criar essas funções de verificação previamente estabelecidas, que funcionam de forma simples. Além disso, fazem o uso
muito elegante de uma ferramenta chamada `parametrize`, que é um decorator do pytest, que tem a função de executar um mesmo teste
múltiplas vezes com uma bateria de inputs pré-definidos.
