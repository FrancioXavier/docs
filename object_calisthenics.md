# Object Calisthenics

## O que é?

Object Calisthenics são um conjunto de regras com o objetivo de, a partir delas, o desenvolvedor conseguir criar um código mais limpo e de acordo com as regras da Programação Orientada a Objetos.

IMPORTANTE: Essas regras não devem ser seguidas cegamente. São exercícios para que, ao tentar executar, consiga de forma intuitiva alcançar uma qualidade maior no seu código.


## 1. Não use ELSE

 - Por que? 
    Com o else, seu código fica menos legível por conta da quebra na linha de raciocínio durante a leitura. Exemplo: ao entrar no if, vai executar toda uma lógica e lá no final do código vai estar o else, seguindo uma condição estabelecida lá no começo do arquivo.

 - Como evitar o else: 

    A que eu mais uso são early returns:

    ```python
        def emptying_tank(self, water_needed: int) -> int:
            if water_needed >= self.capacity:
                self.capacity = 0
                return water_needed - self.capacity
            
            self.capacity -= water_needed
            return 0
    ```
    Nesse exemplo, evito utilizar um else retornando previamente dentro do meu if.

    Outro exemplo é mapeando as ações:

    ```python
        def acao_adicionar():
            print("Adicionando item...")

        def acao_remover():
            print("Removendo item...")

        def acao_padrao():
            print("Ação inválida.")

        acoes = {
            "adicionar": acao_adicionar,
            "remover": acao_remover,
        }

        comando = "editar"

        funcao_a_executar = acoes.get(comando, acao_padrao)
        funcao_a_executar()
    ```
    Aqui, no lugar de colocar else, else if e else, apenas uso o dicionário para chamar as ações por meio de um comando

## 2. Transforme os Primitivos e strings em objetos

- Por que?
    Objetiva facilitar a adição de validação, comportamento e refatoração desses atributos que utilizam tipos primitivos.

- Exemplo:

    ```php
        class CPF {
            public $cpf;

            public function __construct(string $cpf) {
                $this->cpf = $cpf;
            }

            public function validate() {
                //Implementação
            }
        }

        class Pessoa {
            public $cpf;

            public function __construct(CPF $cpf) {
                $this->cpf = $cpf;
            }
        }
    ```

    Pronto! Agora o a classe Pessoa tem um atributo cpf que pode ter diferentes comportamentos ou refatorações e todas as instâncias continurão atualizadas

## 3. Não abreviar

- Por que?
    Tá na cara, né? Outras pessoas precisam entender seu código e os nomes são um dos principais fatores para que alguém entenda o que está acontecendo no seu código.

- Exemplo:
```go
func haveNegativeCycle(digraph *Digraph) ([]int, error) {
	cost, _ := bellmanFord(digraph)

	for _, edge := range digraph.edges {
		if cost[edge.from]+edge.weight < cost[edge.destination] && cost[edge.from] != 1000000 {
			return nil, fmt.Errorf("negative cycle detected")
		}
	}

	return cost, nil
}

func haveNegativeCycle(d *Digraph) ([]int, error) {
	x, _ := bellmanFord(d)

	for _, y := range d.edges {
		if x[y.from]+y.weight < x[y.destination] && x[y.from] != 1000000 {
			return nil, fmt.Errorf("negative cycle detected")
		}
	}

	return x, nil
}
```

## 4. Mantenha as entidades pequenas

- Por que?
    Assim, aqui a gente começa a entrar em uma parte mais sobre tentar do que seguir necessariamente. Mas você deve manter suas classes seguindo o SRP (Single Responsability Principle), ou seja, sua classe deve ter apenas uma responsabilidade. Ao tentar manter as entidades pequenas, você se força a seguir esse SRP e modulariza mais seu código.

- Por exemplo, se sua classe pedido tem vários métodos como despachar, enviar email e processar pagamento, você pode modularizar isso, transformar em outras classes, facilitar futuras manutenções e melhorar a capacidade de reutilizar seu código.


## 5. Não mais do que dois atributos

- Por que?
    Tente ter o mínimo de atributos possível e você vai ver várias possibilidades de modularização no seu código. De novo, nem sempre é pra seguir ao pé da letra, então entenda sua regra de negócio, abstraia ela e tente executar até onde der.

## 6. Utilize classes para coleções

- Por que?
    No mesmo sentido da regra de não utilizar tipos primitivos, o mesmo vale para listas e outros tipos de coleções: Ao encapsular essa lista em um objeto próprio você pode manipular, validar e tratar de forma isolada essa estrutura, facilitando manutenção e reutilização de código.

- Exemplo
```java
class ItensPedido {
    private List<Item> itens = new ArrayList<>();

    public void adicionar(Item item) {
        itens.add(item);
    }

    public int quantidade() {
        return itens.size();
    }

    public List<Item> getItens() {
        return Collections.unmodifiableList(itens);
    }
}

class Pedido {
    private ItensPedido itensPedido;

    public Pedido(ItensPedido itensPedido) {
        this.itensPedido = itensPedido;
    }

    public void adicionarItem(Item item) {
        itensPedido.adicionar(item);
    }
}
```

## 7. Apenas um nível de identação por método

- Por que?
Essa regra visa que você transforme aquele método gigante em vários métodos menores e mais simples, melhorando a legibilidade e diminuindo a responsabilidade de cada método, também facilitando a manutenção.

## 8. Apenas um ponto por linha

- Por que?
Aqui o objetivo principal é o manter o encapsulamento. Não converse com estranhos. Ao utilizar uma sequência de métodos encadeados, você tá acessando dados que sua entidade não deveria acessar, que não fazem parte do papel que aquela classe deveria ter, portanto a regra.

- Exemplo:
```php
// Use
class Pedido {
    private $cliente;

    public function __construct(Cliente $cliente) {
        $this->cliente = $cliente;
    }

    public function getCidadeDoCliente() {
        return $this->cliente->getCidade();
    }
}

class Cliente {
    private $cidade;

    public function __construct($cidade) {
        $this->cidade = $cidade;
    }

    public function getCidade() {
        return $this->cidade;
    }
}

// no lugar de
$cidade = $pedido->getCliente()->getCidade();
```

## 9. Não utilize getters e setters (Diga, não pergunte)

- Por que?
Ao utilizar muitos getters e setters, você prejudica o encapsulamento alterando o estado de uma classe fora dela. A regra se chama "Tell, don't ask" pois você deve ao máximo utilizar o próprio objeto para modificar o estado e não utilizar vários getters, alterar e depois utilizar setters, alterando o estado por fora da classe.
