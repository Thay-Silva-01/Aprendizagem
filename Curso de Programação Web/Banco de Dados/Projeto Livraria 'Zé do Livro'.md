

# Desafio: A Pequena Livraria do Seu Zé
## O Cenário:

O senhor José é um apaixonado por livros que finalmente realizou o sonho de abrir sua própria livraria, a "Livraria do Seu Zé". No início, ele controlava tudo em cadernos: o que comprava, de quem comprava, o que vendia e para quem vendia.

Porém, com o crescimento do negócio, os cadernos se tornaram uma bagunça. Ele não consegue mais responder perguntas simples como:

"Quantos exemplares de 'O Cortiço' eu ainda tenho em estoque?"

"Quem foi o autor mais vendido no mês passado?"

"Qual meu fornecedor com o melhor preço para os livros da Jane Austen?"

"Quem é meu cliente mais fiel?"

Seu Zé está perdendo vendas e comprando mal. Ele precisa de um sistema para organizar sua empresa.

### 📋 O Problema:
Você foi contratado(a) para modelar o banco de dados que será o coração desse novo sistema.

### 💡 Sua Missão (Parte 1 - Modelagem):
1 - Com base na narrativa, identifique as entidades (as "coisas" sobre as quais precisamos guardar informação), seus atributos e como elas se relacionam.

2 - Quais são as principais entidades envolvidas? (ex: Livro, Cliente, etc.)

3 - Quais informações (atributos) precisamos guardar sobre cada uma?

4 - Como essas entidades se conectam? (ex: Um Cliente faz Várias Vendas)

### 🗄️ Sua Missão (Parte 2 - Implementação):
1 - Agora, usando SQL, crie as tabelas para transformar seu modelo em realidade. Pense nos comandos CREATE TABLE e nas restrições (PRIMARY KEY, FOREIGN KEY, NOT NULL).

### 🛠️ Sua Missão (Parte 3 - Consulta):
1 - Escreva consultas SQL para responder às dúvidas do Seu Zé:

2 - Listar todos os livros com menos de 5 unidades em estoque.

3 - Descobrir o autor com mais livros vendidos no último mês.

4 - Encontrar o nome e telefone do cliente que mais comprou no último semestre.

---

# Resolução

### 💡 Sua Missão (Parte 1 - Modelagem):

1 - Com base na narrativa, identifique as entidades (as "coisas" sobre as quais precisamos guardar informação), seus atributos e como elas se relacionam.

Tabelas (Entidades) : Clientes, Livro, Autor, Item venda, Livro Autor, Fornecedores, Estoque, Pagamento, Cupom Desconto

2 - Quais são as principais entidades envolvidas? (ex: Livro, Cliente, etc.)

Tabelas (Entidades) : Clientes, Acervo, Vendas, Fornecedores, Estoque

Autor ←→ Livro ←→ Estoque ←→ Fornecedor

↓
ItemVenda ←→ Venda ←→ Cliente
↓             ↓

Pagamento     CupomDesconto

- 3 - Quais informações (atributos) precisamos guardar sobre cada uma?
    
    **Tabela: Livro**  
    
    - id_livro: identificador único do livro
    - titulo: nome do livro
    - isbn: código internacional de identificação
    - genero: categoria literária
    - ano_publicacao: ano em que foi publicado
    - editora: nome da editora responsável
    - preco: valor de venda
    - estoque: quantidade disponível
    - formato: tipo do livro (físico, ebook, audiobook)
    - idioma: idioma em que o livro está escrito
    
    **Tabela: Autor**  
    
    - id_autor: identificador único do autor
    - nome: nome completo
    - data_nascimento: data de nascimento
    - nacionalidade: país de origem
    - biografia: texto descritivo sobre o autor
    
    **Tabela: LivroAutor**  
    
    - id_livro: referência ao livro
    - id_autor: referência ao autor
    
    **Tabela: Cliente**  
    
    - id_cliente: identificador único do cliente
    - nome: nome completo
    - cpf: documento de identificação
    - email: contato eletrônico
    - telefone: número de telefone
    - endereco: endereço completo
    - data_cadastro: data em que foi registrado
    
    **Tabela: ItemVenda**  
    
    - id_item: identificador único do item
    - id_venda: referência à venda
    - id_livro: referência ao livro vendido
    - quantidade: quantidade vendida
    - preco_unitario: preço do livro no momento da venda
    - desconto: percentual de desconto aplicado
    
    **Tabela: Fornecedor**  
    
    - id_fornecedor: identificador único do fornecedor
    - nome: nome da empresa ou pessoa
    - cnpj: cadastro nacional de pessoa jurídica
    - email: contato eletrônico
    - telefone: número de telefone
    - endereco: localização física
    - data_cadastro: data de registro no sistema
    
    **Tabela: Estoque**  
    
    - id_estoque: identificador da movimentação
    - id_livro: livro movimentado
    - id_fornecedor: fornecedor responsável
    - tipo_movimento: entrada ou saída
    - quantidade: quantidade movimentada
    - data_movimento: data da movimentação
    - observacao: comentários adicionais
    
    **Tabela: Pagamento**  
    
    - id_pagamento: identificador do pagamento
    - id_venda: venda associada
    - forma_pagamento: cartão, pix, boleto, dinheiro
    - valor_pago: valor total pago
    - data_pagamento: data do pagamento
    - status: confirmado, pendente, cancelado
    
    **Tabela: CupomDesconto**  
    
    - id_cupom: identificador do cupom
    - codigo: código promocional
    - descricao: explicação do benefício
    - percentual: percentual de desconto
    - data_validade: data de expiração
    - ativo: indica se está disponível para uso

4 - Como essas entidades se conectam? (ex: Um Cliente faz Várias Vendas)

Cliente (1) → (N) Venda

Venda (1) → (N) ItemVenda

ItemVenda (N) → (1) Livro

Livro (N) → (N) Autor (via LivroAutor)

Livro (1) → (N) Estoque

Fornecedor (1) → (N) Estoque

Venda (1) → (1) Pagamento

CupomDesconto (1) → (N) Venda (opcional)

### 🗄️ Sua Missão (Parte 2 - Implementação):
1 - Agora, usando SQL, crie as tabelas para transformar seu modelo em realidade. Pense nos comandos CREATE TABLE e nas restrições (PRIMARY KEY, FOREIGN KEY, NOT NULL).

#### Tabela: Livro 

`CREATE TABLE Livro (
    id_livro INT PRIMARY KEY,
    titulo VARCHAR(100),
    isbn VARCHAR(20) UNIQUE,
    genero VARCHAR(50),
    ano_publicacao INT,
    editora VARCHAR(100),
    preco DECIMAL(10,2),
    estoque INT,
    formato VARCHAR(20),
    idioma VARCHAR(30)
);`

| Campo | Tipo | Descrição |
| --- | --- | --- |
| id_livro | INT | Identificador único do livro |
| titulo | VARCHAR(100) | Título do livro |
| isbn | VARCHAR(20) | Código ISBN único |
| genero | VARCHAR(50) | Gênero literário |
| ano_publicacao | INT | Ano de publicação |
| editora | VARCHAR(100) | Nome da editora |
| preco | DECIMAL(10,2) | Preço de venda |
| estoque | INT | Quantidade disponível |
| formato | VARCHAR(20) | Tipo: físico, ebook, audiobook |
| idioma | VARCHAR(30) | Idioma do livro |

#### Tabela: Autor

`CREATE TABLE Autor (
    id_autor INT PRIMARY KEY,
    nome VARCHAR(100),
    data_nascimento DATE,
    nacionalidade VARCHAR(50),
    biografia TEXT
);`

| Campo | Tipo | Descrição |
| --- | --- | --- |
| id_autor | INT | Identificador único do autor |
| nome | VARCHAR(100) | Nome completo |
| data_nascimento | DATE | Data de nascimento |
| nacionalidade | VARCHAR(50) | País de origem |
| biografia | TEXT | Texto biográfico |

#### Tabela: Livro_Autor 

`CREATE TABLE Autor (
    id_autor INT PRIMARY KEY,
    nome VARCHAR(100),
    data_nascimento DATE,
    nacionalidade VARCHAR(50),
    biografia TEXT
);`

| Campo | Tipo | Descrição |
| --- | --- | --- |
| id_livro | INT | Referência ao livro |
| id_autor | INT | Referência ao autor |

#### Tabela: Cliente

`CREATE TABLE Cliente (
    id_cliente INT PRIMARY KEY,
    nome VARCHAR(100),
    cpf VARCHAR(14) UNIQUE,
    email VARCHAR(100),
    telefone VARCHAR(20),
    endereco VARCHAR(200),
    data_cadastro DATE
);`

| Campo | Tipo | Descrição |
| --- | --- | --- |
| id_cliente | INT | Identificador único do cliente |
| nome | VARCHAR(100) | Nome completo |
| cpf | VARCHAR(14) | Documento de identificação |
| email | VARCHAR(100) | Contato eletrônico |
| telefone | VARCHAR(20) | Número de telefone |
| endereco | VARCHAR(200) | Endereço completo |
| data_cadastro | DATE | Data de registro |

#### Tabela: Item_Venda

`CREATE TABLE ItemVenda (
    id_item INT PRIMARY KEY,
    id_venda INT,
    id_livro INT,
    quantidade INT,
    preco_unitario DECIMAL(10,2),
    desconto DECIMAL(5,2),
    FOREIGN KEY (id_venda) REFERENCES Venda(id_venda),
    FOREIGN KEY (id_livro) REFERENCES Livro(id_livro)
);`

| Campo | Tipo | Descrição |
| --- | --- | --- |
| id_item | INT | Identificador único do item |
| id_venda | INT | Referência à venda |
| id_livro | INT | Referência ao livro vendido |
| quantidade | INT | Quantidade de exemplares vendidos |
| preco_unitario | DECIMAL(10,2) | Preço do livro na venda |
| desconto | DECIMAL(5,2) | Percentual de desconto aplicado |

#### Tabela Fornecedor

`CREATE TABLE Fornecedor (
    id_fornecedor INT PRIMARY KEY,
    nome VARCHAR(100),
    cnpj VARCHAR(18),
    email VARCHAR(100),
    telefone VARCHAR(20),
    endereco VARCHAR(200),
    data_cadastro DATE
);`

| Campo | Tipo | Descrição |
| --- | --- | --- |
| id_fornecedor | INT | Identificador único do fornecedor |
| nome | VARCHAR(100) | Nome da empresa ou pessoa |
| cnpj | VARCHAR(18) | Cadastro Nacional de Pessoa Jurídica |
| email | VARCHAR(100) | Contato eletrônico |
| telefone | VARCHAR(20) | Número de telefone |
| endereco | VARCHAR(200) | Localização física |
| data_cadastro | DATE | Data de registro |

#### Tabela: Estoque

`CREATE TABLE Estoque (
    id_estoque INT PRIMARY KEY,
    id_livro INT,
    id_fornecedor INT,
    tipo_movimento VARCHAR(10),
    quantidade INT,
    data_movimento DATE,
    observacao TEXT,
    FOREIGN KEY (id_livro) REFERENCES Livro(id_livro),
    FOREIGN KEY (id_fornecedor) REFERENCES Fornecedor(id_fornecedor)
);`

| Campo | Tipo | Descrição |
| --- | --- | --- |
| id_fornecedor | INT | Identificador único do fornecedor |
| nome | VARCHAR(100) | Nome da empresa ou pessoa |
| cnpj | VARCHAR(18) | Cadastro Nacional de Pessoa Jurídica |
| email | VARCHAR(100) | Contato eletrônico |
| telefone | VARCHAR(20) | Número de telefone |
| endereco | VARCHAR(200) | Localização física |
| data_cadastro | DATE | Data de registro |

#### Tabela: Pagamento

`CREATE TABLE Pagamento (
    id_pagamento INT PRIMARY KEY,
    id_venda INT,
    forma_pagamento VARCHAR(20),
    valor_pago DECIMAL(10,2),
    data_pagamento DATE,
    status VARCHAR(20),
    FOREIGN KEY (id_venda) REFERENCES Venda(id_venda)
);`

| Campo | Tipo | Descrição |
| --- | --- | --- |
| id_pagamento | INT | Identificador do pagamento |
| id_venda | INT | Venda associada |
| forma_pagamento | VARCHAR(20) | Cartão, Pix, boleto, dinheiro |
| valor_pago | DECIMAL(10,2) | Valor total pago |
| data_pagamento | DATE | Data do pagamento |
| status | VARCHAR(20) | Confirmado, pendente, cancelado |

#### Tabela: Cupom_Desconto

`CREATE TABLE CupomDesconto (
    id_cupom INT PRIMARY KEY,
    codigo VARCHAR(20) UNIQUE,
    descricao TEXT,
    percentual DECIMAL(5,2),
    data_validade DATE,
    ativo BOOLEAN
);`

| Campo | Tipo | Descrição |
| --- | --- | --- |
| id_cupom | INT | Identificador do cupom |
| codigo | VARCHAR(20) | Código promocional |
| descricao | TEXT | Explicação do benefício |
| percentual | DECIMAL(5,2) | Percentual de desconto |
| data_validade | DATE | Data de expiração |
| ativo | BOOLEAN | Está disponível para uso? |






