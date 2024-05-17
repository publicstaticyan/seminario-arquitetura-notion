# Notion: Análise arquitetural do sistema

<img align="left" height="64px" alt="Logo" src="/img/notion_logo.png"/>

**Notion** \
<small>by Notion Labs Inc</small> \
<small>Lançado em 2016 </small> \
Suportado em `Windows`, `macOS`, `Android`, `iOS`, `Linux`, `Navegadores Web`

Notion é um software de produtividade que permite aos usuários combinar notas, tarefas, wikis e bancos de dados em um único lugar. É usado para organizar projetos pessoais ou de trabalho, tomar notas, criar listas de tarefas, gerenciar equipes e muito mais.

As principais funcionalidades do Notion incluem a capacidade de criar e editar notas, listas de verificação, tabelas e quadros Kanban. Ele também oferece modelos personalizáveis, permitindo que os usuários criem páginas que se adequem às suas necessidades específicas. Além disso, o Notion suporta colaboração em tempo real, permitindo que várias pessoas trabalhem no mesmo projeto simultaneamente.

## Caracterizção

### Influências sobre a decisão arquitetural

* **Número de acessos simultâneos:**
  Foi preciso considerar no projeto do Notion a grande carga de acessos simultâneos esperados e meios de otimizar a performance diante desse desafio. Para isso, é necessário a implementação de balanceamento de carga e estratégias como 're-sharding' para distribuir a carga no banco de dados.

![Processo de Re-Sharding](img/resharding_process.png)
  
* **Número de clientes:**
  À medida que a base de usuários cresce, o sistema precisa ser capaz de escalar de forma eficaz para lidar com o aumento do tráfego e dos requisitos de armazenamento de dados. Para isso, a implementação de uma arquitetura de microsserviços em nuvem permite escalabilidade vertical de partes independentes do sistema conforme necessário.
  
* **Requisitos de segurança:**
  O Notion lida com dados confidenciais do usuário e, portanto, precisa ser projetado medidas de segurança da informação. O uso de criptografia para dados armazenados e em trânsito e a implementação de mecanismos seguros de autenticação e autorização são estratégias utilizadas para reduzir vulnerabilidades.


## Descrição geral da arquitetura
O sistema do Notion foi projetado para ser flexível e adaptável a uma variedade de usos, desde o gerenciamento de projetos até a organização de tarefas pessoais e profissionais.

A arquitetura é baseada em um modelo de dados relacional, que permite a criação de diferentes tipos de conteúdo (como texto, imagens, listas de verificação, etc.) e a relação entre eles. Isso é feito através de "blocos", que são as unidades básicas de conteúdo no Notion. Cada bloco pode conter outros blocos, permitindo a criação de estruturas complexas e hierárquicas de conteúdo.

Além disso, um aspecto importante da arquitetura do Notion é a sua API, que permite a integração com outras ferramentas e serviços. A API do Notion usa o protocolo HTTP e o formato de dados JSON, padrões amplamente utilizados na web.

Em termos de tecnologias utilizadas, o Notion é construído principalmente utilizando várias bibliotecas e frameworks JavaScript, incluindo React e Redux.

### Histórico do sistema
...

### Decisões de projeto
...

### Tech stack
...

---

## Referências
**Arka Ganguli, Tanner Johnson, Ben Kraft, Nathan Northcutt**.
The Great Re-shard: adding Postgres capacity (again) with zero downtime. Disponível em [https://www.notion.so/blog/the-great-re-shard].

**Jake Teton-Landis**.
The data model behind Notion's flexibility. Disponível em [https://www.notion.so/blog/data-model-behind-notion].



