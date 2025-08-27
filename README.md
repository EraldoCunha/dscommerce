# DSCommerce

[![NPM](https://img.shields.io/npm/l/react)](https://github.com/EraldoCunha/dscommerce/blob/main/LICENSE)

# 1. Premissas

Deseja-se fazer um sistema para ser utilizado em cursos da DevSuperior no processo de aprendizado dos alunos. Para isto, a concepção do sistema partiu das seguintes premissas:

- Deve ser um sistema que possua um modelo de domínio relativamente simples, porém abrangente, ou seja, que explore vários tipos de relacionamentos entre as entidades de negócio (muitos-para-um, muitos-para-muitos, etc.).
- O sistema deve possibilitar a aplicação de vários conhecimentos importantes das disciplinas de fundamentos.
- O sistema deve conter as principais funcionalidades que se espera de um profissional iniciante deve saber construir, tais como telas de cadastro e fluxos de caso de uso.

---

# 2. Visão geral do sistema

O sistema deve manter um cadastro de usuário, produtos e suas categorias. Cada usuário possui nome, e-mail, telefone, data de nascimento e uma senha de acesso.  

Os dados dos produtos são: nome, descrição, preço e imagem.  

O sistema deve apresentar um catálogo de produtos, os quais podem ser filtrados pelo nome do produto. A partir desse catálogo, o usuário pode selecionar um produto para ver seus detalhes e para decidir se o adiciona a um carrinho de compras.  

O usuário pode incluir e remover itens do carrinho de compras, bem como alterar as quantidades de cada item.  

Uma vez que o usuário decida encerrar o pedido, o pedido deve então ser salvo no sistema com o status de **"aguardando pagamento"**.  

Os dados de um pedido são:
- Instante em que ele foi salvo,
- Status do pedido,
- Lista de itens, onde cada item se refere a um produto e sua quantidade no pedido.  

O status de um pedido pode ser:
- **Aguardando pagamento**
- **Pago**
- **Enviado**
- **Entregue**
- **Cancelado**  

Quando o usuário paga por um pedido, o instante do pagamento deve ser registrado.  

Os usuários do sistema podem ser **clientes** ou **administradores**, sendo que todo usuário cadastrado por padrão é cliente.  

### Permissões:
- **Usuários não identificados**: podem se cadastrar no sistema, navegar no catálogo de produtos e no carrinho de compras.
- **Clientes**: podem atualizar seu cadastro no sistema, registrar pedidos e visualizar seus próprios pedidos.
- **Administradores**: possuem acesso à área administrativa onde podem acessar os cadastros de usuários, produtos e categorias.

---

# 3. Protótipos de tela

Protótipos disponíveis no Figma:  
🔗 [DSCommerce - Figma](https://www.figma.com/file/ZrGNVNG0kZL6txDv4G8P6s/DSCommerce)

---

# 4. Modelo conceitual

Este é o modelo conceitual do sistema **DSCommerce**.

### Considerações:

- Cada **item de pedido** (`OrderItem`) corresponde a um produto no pedido, com uma quantidade. O preço também é armazenado no item de pedido por questões de histórico (se o preço do produto mudar no futuro, o preço do item de pedido continua registrado com o preço real que foi vendido na época).
- Um usuário pode ter um ou mais **"roles"**, que são os perfis de acesso deste usuário no sistema (`client`, `admin`).

![Modelo Conceitual](https://github.com/EraldoCunha/assets/blob/main/dscommerce/Modelo%20conceitual.PNG)

---

# 5. Casos de uso (visão geral)

## Funcionalidades do sistema:

| Caso de Uso        | Visão Geral | Acesso |
|:--------------------|:------------|:--------|
| **Manter produtos** | CRUD de produtos, podendo filtrar itens pelo nome | Somente Admin |
| **Manter categorias** | CRUD de categorias, podendo filtrar itens pelo nome | Somente Admin |
| **Manter usuários** | CRUD de usuários, podendo filtrar itens pelo nome | Somente Admin |
| **Gerenciar carrinho** | Incluir e remover itens do carrinho de compras, bem como alterar as quantidades do produto em cada item | Público |
| **Consultar catálogo** | Listar produtos disponíveis, podendo filtrar produtos pelo nome | Público |
| **Sign up** | Cadastrar-se no sistema | Público |
| **Login** | Efetuar login no sistema | Público |
| **Registrar pedido** | Salvar no sistema um pedido a partir dos dados do carrinho de compras informado | Usuário logado |
| **Atualizar perfil** | Atualizar o próprio cadastro | Usuário logado |
| **Visualizar pedidos** | Visualizar os pedidos que o próprio usuário já fez | Usuário logado |
| **Registrar pagamento** | Salvar no sistema os dados do pagamento de um pedido | Somente Admin |
| **Reportar pedidos** | Relatório de pedidos, podendo ser filtrados por data | Somente Admin |

![Modelo Conceitual](https://github.com/EraldoCunha/assets/blob/main/dscommerce/Conceito%20de%20uso.PNG)

## **Atores**

| Ator | Responsabilidade |
|:------|:----------------|
| **Usuário anônimo** | Pode realizar casos de uso das áreas públicas do sistema, como catálogo, carrinho de compras, login e sign up. |
| **Cliente** | Responsável por manter seus próprios dados pessoais no sistema e pode visualizar o histórico dos seus pedidos. Todo usuário cadastrado por padrão é um Cliente. |
| **Admin** | Responsável por acessar a área administrativa do sistema com cadastros e relatórios. Admin também pode fazer tudo que Cliente faz. |

---

# 6. Casos de uso (detalhamento)

## **Consultar catálogo**
| Atores | Usuário anônimo, Cliente, Admin |
|:------|:----------------|
| Precondições | - |
| Pós-condições | - |
| Visão geral | Listar produtos disponíveis, podendo filtrar produtos pelo nome|


### **Cenário principal de sucesso**
>1. [OUT] O sistema exibe uma listagem paginada de nome, imagem e preço dos produtos, ordenada por nome.
>
>2. [IN] O usuário informa, opcionalmente, parte do nome de um produto.
>   
>3. [OUT] O sistema exibe a listagem atualizada.

| Informação complementar |
|:----------------------|
| O número padrão de registros por página deve ser **12**. Como a listagem é paginada, o usuário pode navegar nas próximas páginas. |

## **Manter produtos**

| Atores            | Admin                                       |
|:-------------------|:---------------------------------------------|
| Precondições      | Usuário logado                             |
| Pós-condições     | -                                          |
| Visão geral       | CRUD de produtos, podendo filtrar itens pelo nome |

### **Cenário principal de sucesso**

>1. Executar caso de uso: **Consultar catálogo**.
>   
>2. O admin seleciona uma das opções:
> 
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.1. Variante Inserir
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.2. Variante Atualizar
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.3. Variante Deletar

### **Cenário alternativos: variantes**

>**2.1. Variante Inserir** 
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.1.1. **[IN]** O admin informa nome, preço, descrição, URL da imagem e categorias do produto.
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
>
>**2.2. Variante Atualizar**
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.2.1. **[IN]** O admin seleciona um produto para editar.
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.2.2. **[OUT]** O sistema informa nome, preço, descrição, URL da imagem e categorias do produto selecionado.
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.2.3. **[IN]** O admin informa novos valores para nome, preço, descrição, URL da imagem e categorias do produto selecionado.
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
>
>**2.3. Variante Deletar**
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.3.1. **[IN]** O admin seleciona um produto para deletar.


### **Cenário alternativos: exceções**

>2.1.1a. Dados inválidos
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.1.1a.1. [OUT] O sistema informa os erros nos campos inválidos [1].
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.1.1a.2. Vai para passo 2.1.1.
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
>
>2.2.3a. Dados inválidos
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.2.3a.1. [OUT] O sistema informa os erros nos campos inválidos [1].
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.2.3a.2. Vai para passo 2.2.1.
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
>
>2.2.3b. Id não encontrado
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.2.3b.1. [OUT] O sistema informa que o id não existe.
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.2.3b.2. Vai para passo 2.2.1.
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
>    
>2.3.1a. Id não encontrado
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.3.1a.1. [OUT] O sistema informa que o id não existe.
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.3.1a.2. Vai para passo 2.3.1.
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
>     
>2.3.1b. Integridade referencial violada
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.3.1b.1. [OUT] O sistema informa que a deleção não pode ser feita porque viola a integridade referencial dos dados.
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.3.1b.2. Vai para passo 2.3.1.


### **Informações complementares**
>[1] Validação dos dados:
>
>* Nome: deve ter entre 3 e 80 caracteres
>
>* Preço: deve ser positivo
>
>* Descrição: não pode ter menos que 10 caracteres
>
>* Deve haver pelo menos 1 categoria

## **Login**
| **Atores** | **Usuário anônimo** |
|:------|:----------------|
| Precondições | - |
| Pós-condições | Usuário logado |
| Visão geral | Efetuar login no sistema |

### **Cenário principal de sucesso**
>1. [IN] O usuário anônimo informa suas credenciais (nome e senha).
>
>2. [OUT] O sistema informa um token válido.


### **Cenário alternativos: exceções**
>1a. Credenciais inválidas
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1a.1. [OUT] O sistema informa que as credenciais são inválidas.
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1a.2. Vai para passo 1.

## **Gerenciar carrinho**
| **Atores** | **Usuário anônimo** |
|:------|:----------------|
| Precondições | - |
| Pós-condições | - |
| Visão geral | Incluir e remover itens do carrinho de compras, bem como alterar as quantidades do produto em cada item |


### **Cenário principal de sucesso**
>1. Executar caso de uso: **Consultar catálogo**.
>
>2. [IN] O Usuário anônimo seleciona um produto.
>
>3. [OUT] O sistema informa nome, preço, descrição, imagem, e nomes das categorias do produto selecionado.
>
>4. [IN] O Usuário anônimo informa que deseja adicionar o produto ao carrinho.
>
>5. [OUT] O sistema informa os dados do carrinho de compras [1].
>
>6. [IN] O Usuário anônimo informa, opcionalmente, que deseja incrementar ou decrementar a quantidade de um item no carrinho de compras.
>
>7. [ OUT] O sistema informa os dados atualizados do carrinho de compras [1].

   
Informações complementares
>**[1] Dados do carrinho de compras**:
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- lista de itens de carrinho
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- valor total do carrinho de compras
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dados do item de carrinho**:
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- id do produto
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- nome do produto
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- preço do produto
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- quantidade
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- subtotal


## **Registrar pedido**
| **Atores** | **Cliente** |
|:------|:----------------|
| Precondições | Usuário logado e Carrinho de compras não vazio|
| Pós-condições | Carrinho de compras vazio |
| Visão geral | Salvar no sistema um pedido a partir dos dados do carrinho de compras informado. |


### **Cenário principal de sucesso**
>1. [IN] O cliente informa os dados do carrinho de compras [1].
>
>2. [OUT] O sistema informa os dados do carrinho de compras[1] e o id do pedido.


### **Informações complementares**
>[1] Dados do carrinho de compras: vide caso de uso **Gerenciar carrinho**.

---

# Tecnologias utilizadas

- **Java**
- **Spring Boot**
- **Spring Starter Web**
- **Spring Security**
- **Spring JWT**
- **Oauth2**
- **JPA / Hibernate**
- **H2 Database**
- **Maven**
- **Bean Validation**
- **Postman**

# Autor

Eraldo José de Almeida Cunha

<a href="https://www.linkedin.com/in/eraldojacunha/" target="_blank"><img src="https://img.shields.io/badge/-LinkedIn-%230077B5?style=for-the-badge&logo=linkedin&logoColor=white" target="_blank"></a>
