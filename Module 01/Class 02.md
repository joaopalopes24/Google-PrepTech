# Aula 02

## Listas Encadeadas

##### Playlist de músicas:

- Sem limite de músicas que podem ser adicionadas.
- Músicas podem ser adicionadas em qualquer posição da playlist.
- Usuários podem remover qualquer música da playlist.
- Usuários podem trocar a ordem das músicas.

#### Listas Encadeadas

- Estrutura de dados linear.
- Nós (`nodes`) da lista armazenam dados e uma referência (`next`) para o próximo nó.
- A referência `next` do último nó da lista aponta para `null`.
- O nó inicial da lista geralmente é chamado de `head`.

```bash
HEAD
  ↳ [Nó A: 1] → [Nó B: 2] → [Nó C: 3] → [Nó D: 4] → NULL
```

#### Tipos:

- Singly linked list.
- Doubly linked list.
- Circular linked list.

### Arrays x Listas Encadeadas

#### Arrays

- Tamanho fixo: resizing é “caro”.
- Inserções e remoções podem ser ineficientes (geralmente envolve “shift” nos elementos).
- Acesso aleatório (index).
- Memória previamente alocada para os elementos.
- Acesso sequencial é rápido.

#### Listas Encadeadas

- Tamanho dinâmico.
- Inserções e remoções são eficientes: não envolvem “shifts”.
- Acesso sequencial.
- Memória alocada dinamicamente quando necessário.
- Acesso sequencial é mais lento.

### Operações

- Criação da lista.
- Inserção.
- Remoção.
- Busca.
- Inversão.

#### Criação:

Nós da Lista:

```php
class Node
{
    public function __construct(
        public mixed $data,
        public mixed $next = null,
    ){}
}
```

A lista encadeada é reprensetada por uma referência para o primeiro nó.

```php
class LinkedList
{
    public function __construct(
        public mixed $head = null,
    ){}
}
```

#### Inserção

- Inserção no início.
- Inserção no fim.
- Inserção em uma posição específica.

##### Inserção no Início:

- Criar um novo nó.
- Atualiza a referência `next` do novo nó para apontar para `head`.
- Atualiza a referência `head` da lista para apontar para o nó recém criado.

```bash
HEAD
  ↳ [Nó A] → [Nó B] → [Nó C] → [Nó D] → NULL
```

```bash
HEAD
  ↳ [Nó A] → [Nó B] → [Nó C] → [Nó D] → NULL

[Nó E] → NULL
```

```bash
HEAD
  ↳ [Nó A] → [Nó B] → [Nó C] → [Nó D] → NULL
  ↱
[Nó E]
```

```bash
        [Nó A] → [Nó B] → [Nó C] → [Nó D] → NULL
HEAD   ↱
  ↳ [Nó E]
```

```bash
HEAD
  ↳ [Nó E] → [Nó A] → [Nó B] → [Nó C] → [Nó D] → NULL
```

```php
class LinkedList
{
    public function insertBegin(mixed $data): void
    {
        $newNode = new Node($data);
        
        $newNode->next = $this->head;

        $this->head = $newNode;
    }
}
```

##### Inserção no Fim:

- Criar um novo nó.
- Atualiza o último nó da lista apontar para o novo nó.

```bash
HEAD
  ↳ [Nó A] → [Nó B] → [Nó C] → [Nó D] → NULL
```

```bash
HEAD
  ↳ [Nó A] → [Nó B] → [Nó C] → [Nó D] → NULL

[Nó E] → NULL
```

- Percorrer toda a lista até chegar ao final.

```bash
HEAD
  ↳ [Nó A] → [Nó B] → [Nó C] → [Nó D]
                                  ↳ [Nó E] → NULL
```

```bash
HEAD
  ↳ [Nó A] → [Nó B] → [Nó C] → [Nó D] → [Nó E] → NULL
```

```php
class LinkedList
{
    public function insertEnd(mixed $data): void
    {
        $newNode = new Node($data);

        if ($this->head === NULL) {
            $this->head = $newNode;

            return;
        }

        $aux = $this->head;

        while ($aux->next !== NULL) {
            $aux = $aux->next;
        }
        
        $aux->next = $newNode;
    }
}
```

##### Inserção em uma Posição Específica:

- Criar um novo nó.
- Encontrar o nó na posição anterior a que queremos inserir (vamos chamá-lo de `q`).
- Atualizar a referência `next` do novo nó para apontar para o nó seguinte a `q`.
- Atualizar a referência `next` do nó `q` para apontar para o novo nó.

```bash
HEAD
  ↳ [Nó A] → [Nó B] → [Nó C] → [Nó D] → NULL
```

```bash
HEAD
  ↳ [Nó A] → [Nó B] → [Nó C] → [Nó D] → NULL
                             ↱
                          [Nó E]
```

```bash
HEAD
  ↳ [Nó A] → [Nó B]
                ↳ [Nó C]   ↱ [Nó D] → NULL
                    ↳ [Nó E]
```

```bash
HEAD
  ↳ [Nó A] → [Nó B] → [Nó C] → [Nó E] → [Nó D] → NULL
```

```php
class LinkedList
{
    public function insertAfter(mixed $data, mixed $pos): void
    {
        $newNode = new Node($data);

        $aux = $this->head;

        for ($i = 0; $i < $pos; $i++) {
            if ($aux === NULL) {
                return;
            }

            $aux = $aux->next;
        }

        $newNode->next = $aux->next;
        
        $aux->next = $newNode;
    }
}
```

#### Remoção

- Remoção no início.
- Remoção no fim.
- Remoção em uma posição específica.

##### Remoção no Início:

- Atualizar a referência `head` para apontar para o segundo nó da lista (`head.next`).

```bash
HEAD
  ↳ [Nó A] → [Nó B] → [Nó C] → [Nó D] → NULL
```

```bash
     HEAD
       ↳
[Nó A] → [Nó B] → [Nó C] → [Nó D] → NULL
```

```bash
HEAD
  ↳ [Nó B] → [Nó C] → [Nó D] → NULL
```

```php
class LinkedList
{
    public function removeBegin(): void
    {
        if ($this->head === NULL) {
            return;
        }

        $this->head = $this->head->next;
    }
}
```

##### Remoção no Fim:

- Criar um novo nó.
- Atualiza a referência `next` do penúltimo nó para apontar para `null`.

```bash
HEAD
  ↳ [Nó A] → [Nó B] → [Nó C] → [Nó D] → NULL
```

- Percorre toda a lista até chegar no penúltimo.

```bash
HEAD
  ↳ [Nó A] → [Nó B] → [Nó C]   [Nó D] → NULL
                         ↳ NULL
```

```bash
HEAD
  ↳ [Nó A] → [Nó B] → [Nó C] → NULL
```

```php
class LinkedList
{
    public function removeEnd(): void
    {
        if ($this->head === NULL) {
            return;
        }

        if ($this->head->next === NULL) {
            $this->head = NULL;

            return;
        }

        $aux = $this->head;

        while ($aux->next->next !== NULL) {
            $aux = $aux->next;
        }
        
        $aux->next = NULL;
    }
}
```

##### Remoção em uma Posição Específica:

- Encontrar o nó na posição que queremos deletar (vamos chamá-lo de `q`).
- Atualiza a referência `next` do nó anterior à `q` para apontar para `q.next`.

```bash
HEAD
  ↳ [Nó A] → [Nó B] → [Nó C] → [Nó D] → NULL
```

```bash
HEAD
  ↳ [Nó A] → [Nó B]  [Nó C] → [Nó D] → NULL
                ↳ → → → → → ↱
```

```bash
HEAD
  ↳ [Nó A] → [Nó B] → → → → →  [Nó D] → NULL
```

```php
class LinkedList
{
    public function removePos(mixed $pos): void
    {
        if ($this->head === NULL) {
            return;
        }

        $aux = $this->head;

        for ($i = 0; $i < $pos; $i++) {
            if ($aux === NULL) {
                return;
            }

            $aux = $aux->next;
        }

        if ($aux->next === NULL) {
            return;
        }

        $aux->next = $aux->next->next;
    }
}
```

### Listas Duplamente Encadeada

```bash
HEAD
  ↳ [Nó A] ⇄ [Nó B] ⇄ [Nó C] ⇄ [Nó D] → NULL
 NULL ↲                      ↱
                          [FOOT]
```

### Listas Circulares

```bash
   ↱→→→→→→→→→→→→→→→→→→→→→→→→→↴
[Nó A] ⇄ [Nó B] ⇄ [Nó C] ⇄ [Nó D]
   ↱←←←←←←←←←←←←←←←←←←←←←←←←←↲
```