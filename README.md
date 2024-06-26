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

Inicialmente concebida como um construtor de páginas web, a primeira versão enfrentou dificuldades devido a uma tech-stack instavél e uma abordagem que não correspondia às necessidades dos usuários. Em 2016, Notion foi relançada com a versão 2.0, que introduziu funcionalidades de banco de dados e um foco em fornecer templates simples para facilitar o trabalho dos usuários, o que marcou um ponto de virada para a empresa. Durante esse período, a equipe se mudou para Kyoto para se concentrar no desenvolvimento do produto, ajudando a expandir a equipe de oito para quinhentas pessoas em quatro anos.

A popularidade de Notion cresceu significativamente com a pandemia de 2020, que impulsionou a necessidade de ferramentas de colaboração remota, ajudando a quintuplicar sua base de usuários. Com mais de 30 milhões de usuários em 2023, Notion atraiu uma variedade de empresas e indivíduos criativos, sendo valorizada em mais de $20 bilhões. A capacidade de personalização e a interface intuitiva contribuíram para seu sucesso, além do movimento no-code que facilitou o uso por pessoas sem conhecimento de programação.

### Decisões de projeto

Como dito anteriormente, o Notion adota uma abordagem menos tradicional, organizando os dados em entidades modulares conhecidas como blocos. Esses blocos constituem as unidades fundamentais de informação e facilitam uma estrutura hierárquica versátil, capaz de acomodar vários tipos de dados, incluindo texto, imagens, listas, linhas de dados e até páginas inteiras. Um dos principais motivos para adotar esse uso é o fato que os blocos fornecem flexibilidade que não está disponível nos formatos de documentos tradicionais. Por exemplo, como o conteúdo de uma página não é armazenado diretamente dentro de um bloco de páginas, o mesmo bloco de conteúdo pode ser incluído em diversas páginas ou diversas vezes na mesma página.

O Notion aproveita um banco de dados PostgreSQL hospedado no serviço de nuvem Amazon RDS for PostgreSQL para armazenar seus blocos. No início, o Notion usava um servidor de banco de dados para lidar com todas as solicitações. À medida que o número de usuários aumentava, a equipe da Notion aplicava escalonamento vertical, aumentando a capacidade do servidor virtual para acompanhar o aumento da carga. Eventualmente, mesmo a maior instância de banco de dados disponível da Amazon não foi capaz de lidar com a carga cada vez maior e a equipe da Notion teve que dividir o único servidor de banco de dados em um cluster de servidores. Em julho de 2023, o cluster de banco de dados consistia em 96 servidores de banco de dados. Cada servidor de banco de dados possui 1 banco de dados que é particionado logicamente em 5 fragmentos, com cada fragmento representado como um esquema PostgreSQL.

Os dados são particionados em fragmentos lógicos usando o ID do espaço de trabalho como chave de partição. Isso garante que todos os blocos pertencentes a um espaço de trabalho sejam armazenados no mesmo banco de dados. Esta abordagem permite a utilização de transações e garante consistência no armazenamento e atualização de blocos.

Os client-sides fazem interface com o banco de dados do Notion por meio de um servidor API dedicado, operando em um cluster de servidores web Node.js. A conexão com o banco de dados é gerenciada através do pool de conexões PgBouncer, melhorando o desempenho e a escalabilidade.

### Tech stack

![Tecnologias utilizadas](img/techstack.png)

### Linguagens e Frameworks 

- **Node.js**: Ambiente de execução JavaScript server-side.
- **Next.js**: Framework React para desenvolvimento web.
- **JavaScript**: Linguagem de programação amplamente utilizada para desenvolvimento web.
- **Python**: Linguagem de programação versátil, usada em diversas áreas como ciência de dados e desenvolvimento web.
- **HTML5**: Linguagem de marcação padrão para criação de páginas web.
- **CSS 3**: Linguagem de estilo para descrever a apresentação de documentos HTML.
- **Kotlin**: Linguagem de programação moderna e estática usada principalmente para desenvolvimento Android.
- **Swift**: Linguagem de programação desenvolvida pela Apple para desenvolvimento de aplicativos iOS.
- **TypeScript**: Superconjunto de JavaScript que adiciona tipagem estática ao código.

### Development

- **Babel**: Transpilador JavaScript que permite usar a próxima geração de JavaScript, hoje.
- **Webpack**: Empacotador de módulos JavaScript.
- **AWS Elastic Load Balancing**: Serviço que distribui automaticamente o tráfego de aplicativos por várias instâncias.
- **Docker**: Plataforma para desenvolver, enviar e executar aplicativos em contêineres.

### Libraries

- **React**: Biblioteca JavaScript para construção de interfaces de usuário.
- **jQuery**: Biblioteca JavaScript que simplifica manipulação de HTML, eventos e animações.
- **Moment.js**: Biblioteca JavaScript para manipulação e formatação de datas.
- **Lodash**: Biblioteca JavaScript que fornece utilitários para tarefas de programação comuns.

### Hosting

- **Amazon EC2**: Serviço da AWS que fornece capacidade de computação escalável na nuvem.
- **Amazon Web Services (AWS)**: Plataforma de serviços de computação em nuvem da Amazon.

### Data Stores

- **Apache Spark**: Motor de processamento de dados para análise em tempo real.
- **Apache Flink**: Plataforma de processamento de fluxo de dados.
- **Amazon S3**: Serviço de armazenamento de objetos escalável.
- **dbt**: Ferramenta para transformação de dados no fluxo de trabalho de ETL.
- **PostgreSQL**: Sistema de gerenciamento de banco de dados relacional e objeto-relacional.
- **Memcached**: Sistema de cache distribuído.
- **Hadoop**: Framework para processamento de grandes conjuntos de dados.
- **Kafka**: Plataforma de streaming de eventos.
- **Airflow**: Plataforma para criar, agendar e monitorar fluxos de trabalho.


## Referências
**Arka Ganguli, Tanner Johnson, Ben Kraft, Nathan Northcutt**.
The Great Re-shard: adding Postgres capacity (again) with zero downtime. Disponível em [https://www.notion.so/blog/the-great-re-shard].

**Himalayas app Notion tech stack**
Discover the tools and technologies used to build Notion's products and services. Disponível em [https://himalayas.app/companies/notion/tech-stack]

**Michael slashdev**
Breaking down Notion Tech Stack.  Disponível em [https://slashdev.io/-breaking-down-notions-tech-stack]

**Jake Teton-Landis**.
The data model behind Notion's flexibility. Disponível em [https://www.notion.so/blog/data-model-behind-notion].

**Relbis Labs**
Examining Notion's Backend Architecture. Disponível em [https://labs.relbis.com/blog/2024-04-18_notion_backend].



