# Banco de Dados de Saúde — Trabalho Acadêmico

Projeto desenvolvido para a disciplina de **Banco de Dados Relacional**. O trabalho consiste na modelagem de um banco de dados genérico para uma clínica de saúde e no cadastro de alguns exemplos de dados para demonstrar o funcionamento dos relacionamentos definidos.

## Objetivo

Aplicar conceitos de banco de dados relacionais, incluindo:

- identificação de entidades e atributos;
- definição de chaves primárias e estrangeiras;
- relacionamentos entre tabelas;
- relacionamento muitos-para-muitos;
- normalização e redução de redundâncias;
- regras de integridade dos dados;
- cadastro de dados fictícios para validação do modelo.

## Escopo do trabalho

O banco representa uma estrutura básica de atendimento em saúde, contemplando:

- clínicas;
- funcionários vinculados às clínicas;
- médicos como especialização de funcionários;
- pacientes vinculados a uma clínica e a um médico responsável;
- consultas entre pacientes, médicos e clínicas;
- procedimentos oferecidos pelas clínicas;
- medicamentos;
- receituários vinculados exclusivamente a consultas e medicamentos;
- histórico de eventos clínicos ou administrativos.

O projeto é limitado à **modelagem e ao cadastro de exemplos no banco de dados**. Não faz parte do escopo desenvolver uma aplicação, uma API, uma interface web, um sistema de autenticação ou uma infraestrutura de produção.

## Modelo de dados

As principais regras adotadas são:

1. Uma clínica pode possuir vários funcionários, médicos, pacientes e procedimentos.
2. Todo médico deve estar vinculado a um funcionário e a uma clínica.
3. O paciente possui uma clínica e um médico responsável da mesma clínica.
4. A consulta relaciona uma clínica, um paciente, um médico e, opcionalmente, um procedimento.
5. Uma clínica pode oferecer vários procedimentos, e um procedimento pode ser oferecido por várias clínicas.
6. O receituário depende obrigatoriamente de uma consulta e de um medicamento.
7. O histórico pode estar relacionado a paciente, clínica, médico, procedimento ou consulta.
8. Os dados utilizados para exemplos devem ser fictícios, sem informações reais de pacientes.

## Estrutura do repositório

```text
.
├── README.md
└── Preparação
	├── DBML
	│   └── saude.dbml
	└── Text
		└── Structure.md
```

### Arquivos

- [`Preparação/DBML/saude.dbml`](Preparação/DBML/saude.dbml): modelo das tabelas, atributos, índices e relacionamentos em DBML.
- [`Preparação/Text/Structure.md`](Preparação/Text/Structure.md): descrição das entidades, relacionamentos e regras de integridade.

## Tecnologias e ferramentas

- Banco de dados relacional compatível com o modelo proposto;
- PostgreSQL como referência para os tipos de dados;
- DBML para representação visual e documentação do modelo;
- GitHub para armazenamento e compartilhamento do trabalho.

## Fora do escopo

Não serão implementados neste trabalho:

- sistema completo de prontuário eletrônico;
- autenticação e controle de usuários;
- integração com convênios ou sistemas externos;
- faturamento e controle financeiro;
- criptografia, backups automatizados e monitoramento de produção;
- aplicação web, aplicativo móvel ou API.

## Resultado esperado

Ao final, o repositório deverá apresentar uma modelagem relacional coerente e alguns registros fictícios inseridos no banco, permitindo verificar os relacionamentos entre clínicas, funcionários, médicos, pacientes, consultas, procedimentos, medicamentos, receituários e histórico.

## Observação

Este é um projeto acadêmico com finalidade exclusivamente didática. Os dados de exemplo não representam pessoas ou atendimentos reais.