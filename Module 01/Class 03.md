# Aula 03

## Pilhas

Uma pilha é uma estrutura de dados linear que segue o princípio  LIFO _(Last In, First Out)_, onde o último elemento inserido é o  primeiro a ser removido.

```bash
TOP → [50]
      [6]
      [20]
      [1000]
      [1]
```

#### Operações Básicas:

- `push`: adiciona um elemento ao topo da pilha.
- `pop`: remove o elemento do topo da pilha.
- `top/peek`: acessa o elemento do topo da pilha sem removê-lo.

```php
class Stack
{
    protected array $items = [];

    public function push(mixed $item): void

    public function pop(): mixed

    public function top(): mixed

    public function isEmpty(): bool
}
```

##### PUSH - Adicionando elementos na Pilha

Elementos sempre são adicionados ao topo da pilha. Dizemos que elementos são “empilhados”.

```php
$stack->push(120);
```

```bash
TOP → [50]
      [6]
      [20]
      [1000]
      [1]
```

```bash
TOP → [120]
      [50]
      [6]
      [20]
      [1000]
      [1]
```

##### POP - Removendo elementos na Pilha

Elementos sempre são retirados do topo da pilha. Dizemos que elementos são “desempilhados”.

```php
$stack->pop(120); // Return 120
```

```bash
TOP → [120]
      [50]
      [6]
      [20]
      [1000]
      [1]
```

```bash
TOP → [50]
      [6]
      [20]
      [1000]
      [1]
```

##### TOP - Acessando o Último Elemento Inserido

A função `top` (algumas implementações chamam essa operação de `peek`) acessa o último elemento inserido na pilha, sem removê-lo.

```php
$stack->top(); // Return 50
```

```bash
TOP → [50]
      [6]
      [20]
      [1000]
      [1]
```

#### Implementação

```php
class Stack
{
    protected array $items = [];

    public function push(mixed $item): void
    {
        array_push($this->items, $item);
    }

    public function pop(): mixed
    {
        if ($this->isEmpty()) {
            return NULL;
        }

        return array_pop($this->items);
    }

    public function top(): mixed
    {
        if ($this->isEmpty()) {
            return NULL;
        }

        return $this->items[-1];
    }

    public function isEmpty(): bool
    {
        return count($this->items) === 0;
    }
}
```

##### Perguntas:

1) Na PILHA, Qual é a complexidade em função do tempo para as operações:

- `push(item)`
- `pop()`
- `top()`
- `isEmpty()`

<!-- 
Todas essas operações possuem complexidade O(1) de tempo.

Desde que não cause o redimensionamento do array, que no caso seria O(n).
-->

## Filas

Uma fila é uma estrutura de dados linear que segue o princípio FIFO _(First In, First Out)_, onde o primeiro elemento inserido é o primeiro a ser removido. Ou seja, elementos são removidos na mesma ordem de inserção.

```bash
 [3] [-43] [-9] [14] [133] [5]
  ↑                         ↑
INICIO                     FIM
```

#### Operações Básicas:

- `enqueue`: adiciona um elemento ao final da fila.
- `dequeue`: remove o elemento do início da fila.
- `front/peek`: acessa o elemento do início da fila sem removê-lo.

```php
class Queue
{
    protected array $items = [];

    public function enqueue(mixed $item): void

    public function dequeue(): mixed

    public function front(): mixed

    public function isEmpty(): bool
}
```

##### ENQUEUE - Adicionando elementos na Fila

Elementos sempre são adicionados ao final da fila. Dizemos que elementos são “enfileirados”.

```php
$queue->enqueue(120);
```

```bash
 [3] [-43] [-9] [14] [133] [5]
  ↑                         ↑
INICIO                     FIM
```

```bash
 [3] [-43] [-9] [14] [133] [5] [120]
  ↑                              ↑
INICIO                          FIM
```

##### DEQUEUE - Removendo elementos na Fila

Elementos sempre são removidos do início da fila. Dizemos que elementos são “desenfileirados”.

```php
$queue->dequeue(120); // Return 3
```

```bash
 [3] [-43] [-9] [14] [133] [5] [120]
  ↑                              ↑
INICIO                          FIM
```

```bash
 [-43] [-9] [14] [133] [5] [120]
   ↑                         ↑
INICIO                      FIM
```

##### FRONT - Acessando o Elemento do Ínicio da Fila

A função `front` (algumas implementações chamam essa função é `peek`) acessa o elemento do início da fila, sem removê-lo.

```php
$queue->front(); // Return -43
```

```bash
 [-43] [-9] [14] [133] [5] [120]
   ↑                         ↑
INICIO                      FIM
```

#### Implementação com Array

```php
class Queue
{
    protected array $items = [];

    public function enqueue(mixed $item): void
    {
        array_push($this->items, $item);
    }

    public function dequeue(): mixed
    {
        if ($this->isEmpty()) {
            return NULL;
        }

        return array_shift($this->items);
    }

    public function front(): mixed
    {
        if ($this->isEmpty()) {
            return NULL;
        }

        return $this->items[0];
    }

    public function isEmpty(): bool
    {
        return count($this->items) === 0;
    }
}
```

##### Perguntas:

1) Na FILA, Qual é a complexidade em função do tempo para as operações:

- `enqueue(item)`
- `dequeue()`
- `front()`
- `isEmpty()`

2) O que muda na implementação da PILHA?

<!-- 
Adicionar um elemento ao final do array: O(1).

Verificar se o array está vazio: O(1).

Retornar o último elemento do array: O(1).

Remover do início do array: O(N).
-->

#### Implementação com Lista Encadeada

```php
class Node
{
    public function __construct(
        public mixed $data,
        public mixed $next = NULL,
    ){}
}

class Queue
{
    protected mixed $from = NULL;

    protected mixed $rear = NULL;

    public function enqueue(mixed $item): void
    {
        $newNode = new Node($data);

        if ($this->rear === NULL) {
            $this->front = $this->near = $newNode;
        } else {
            $this->rear->next = $newNode;

            $this->rear = $newNode;
        }
    }

    public function dequeue(): mixed
    {
        if ($this->isEmpty()) {
            return NULL;
        }

        $node = $this->front;

        $this->front = $node->next;

        if ($this->front === NULL) {
            $this->rear = NULL;
        }

        return $node->data;
    }

    public function front(): mixed
    {
        if ($this->isEmpty()) {
            return NULL;
        }

        return $this->front->data;
    }

    public function isEmpty(): bool
    {
        return $this->front === NULL;
    }
}
```

<!-- 
Todas as operações de uma fila implementadas usando lista encadeada tem complexidade O(1) de tempo.
-->