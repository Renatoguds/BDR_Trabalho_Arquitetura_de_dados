# Estrutura genérica do banco de dados de saúde

## Decisões de modelagem

- Uma `clinica` possui funcionários, médicos, pacientes e procedimentos oferecidos.
- `medico` é uma especialização de `funcionario`. Assim, todo médico é funcionário de uma clínica, mas nem todo funcionário é médico.
- Um paciente é vinculado a uma clínica e a um médico responsável. O médico responsável deve pertencer à mesma clínica do paciente.
- `consulta` representa o atendimento realizado por um médico para um paciente. Ela é a origem obrigatória do receituário.
- Um procedimento pode ser oferecido por várias clínicas e realizado várias vezes; por isso, a relação entre clínica e procedimento é N:N.
- Um receituário possui um ou mais medicamentos. Cada item do receituário depende de uma consulta e de um medicamento.
- O histórico registra eventos e pode apontar para paciente, clínica, médico, procedimento ou consulta. Pelo menos um desses vínculos deve existir.

## Entidades e relacionamentos

```text
CLINICA 1 ---- N FUNCIONARIO
FUNCIONARIO 1 ---- 0..1 MEDICO
CLINICA 1 ---- N PACIENTE
MEDICO 1 ---- N PACIENTE
PACIENTE 1 ---- N CONSULTA
MEDICO 1 ---- N CONSULTA
CLINICA 1 ---- N CONSULTA
CLINICA N ---- N PROCEDIMENTO (por CLINICA_PROCEDIMENTO)
CONSULTA 1 ---- N RECEITUARIO
MEDICAMENTO 1 ---- N RECEITUARIO
PACIENTE 1 ---- N HISTORICO (vínculo recomendado)
```

## Tabelas

### `clinica`

Armazena as unidades de atendimento.

- `id_clinica` (PK)
- `nome`
- `cnpj` (UNIQUE)
- `telefone`
- `email`
- `logradouro`, `numero`, `complemento`, `cidade`, `estado`, `cep`
- `ativo`

### `funcionario`

Armazena todos os profissionais e colaboradores vinculados a uma clínica.

- `id_funcionario` (PK)
- `id_clinica` (FK obrigatória para `clinica`)
- `nome`
- `cpf` (UNIQUE)
- `cargo`
- `registro_profissional` (opcional)
- `data_admissao`
- `ativo`

### `medico`

Especialização de funcionário para dados específicos do médico.

- `id_funcionario` (PK e FK para `funcionario`)
- `crm` (UNIQUE)
- `especialidade`

### `paciente`

Mantém o paciente associado à clínica e ao médico responsável.

- `id_paciente` (PK)
- `id_clinica` (FK para `clinica`)
- `id_medico_responsavel` (FK para `medico`)
- `nome`
- `cpf` (UNIQUE)
- `data_nascimento`
- `sexo`
- `telefone`
- `email`
- `endereco`
- `ativo`

Uma chave estrangeira composta (`id_clinica`, `id_medico_responsavel`) deve referenciar a associação do médico com a clínica, evitando que um paciente seja atribuído a médico de outra clínica.

### `procedimento`

Catálogo geral de procedimentos de saúde.

- `id_procedimento` (PK)
- `nome`
- `descricao`
- `duracao_estimada_minutos`
- `ativo`

### `clinica_procedimento`

Tabela associativa que informa quais procedimentos cada clínica oferece.

- `id_clinica` (PK/FK para `clinica`)
- `id_procedimento` (PK/FK para `procedimento`)
- `valor`
- `ativo`

### `consulta`

Registra o atendimento e conecta paciente, médico, clínica e, opcionalmente, um procedimento.

- `id_consulta` (PK)
- `id_clinica` (FK para `clinica`)
- `id_paciente` (FK para `paciente`)
- `id_medico` (FK para `medico`)
- `id_procedimento` (FK opcional para `procedimento`)
- `data_hora`
- `queixa_principal`
- `diagnostico`
- `observacoes`
- `status`

As combinações de clínica, paciente e médico devem ser validadas para que os três pertençam à mesma clínica.

### `medicamento`

Catálogo de medicamentos que podem ser prescritos.

- `id_medicamento` (PK)
- `nome_comercial`
- `principio_ativo`
- `concentracao`
- `forma_farmaceutica`
- `fabricante`
- `ativo`

### `receituario`

Item de receita. Não possui paciente ou médico próprios: esses dados são obtidos pela `consulta`, garantindo que o receituário seja exclusivo de uma consulta.

- `id_receituario` (PK)
- `id_consulta` (FK obrigatória para `consulta`)
- `id_medicamento` (FK obrigatória para `medicamento`)
- `quantidade`
- `posologia`
- `duracao_dias`
- `observacoes`

Uma consulta pode possuir vários itens de receituário; um medicamento pode aparecer em várias receitas.

### `historico`

Registra eventos clínicos ou administrativos relacionados ao atendimento.

- `id_historico` (PK)
- `id_paciente` (FK recomendada para `paciente`)
- `id_clinica` (FK opcional para `clinica`)
- `id_medico` (FK opcional para `medico`)
- `id_procedimento` (FK opcional para `procedimento`)
- `id_consulta` (FK opcional para `consulta`)
- `tipo_evento`
- `descricao`
- `data_hora`

Regra de integridade: pelo menos um entre `id_paciente`, `id_clinica`, `id_medico`, `id_procedimento` e `id_consulta` deve ser preenchido. Quando o histórico for clínico, recomenda-se exigir também `id_paciente`.

## Regras de integridade principais

1. `funcionario.id_clinica` é obrigatório; funcionário não existe sem clínica.
2. `medico.id_funcionario` deve existir em `funcionario`.
3. `paciente.id_clinica` e `paciente.id_medico_responsavel` são obrigatórios.
4. Médico, paciente e consulta devem estar vinculados à mesma clínica.
5. `receituario.id_consulta` e `receituario.id_medicamento` são obrigatórios; não há receituário independente.
6. Exclusões devem usar `RESTRICT` para preservar o histórico. Para desativação, prefira o campo `ativo`.
7. Datas de nascimento não podem ser futuras, e quantidades e durações de medicamentos devem ser maiores que zero.

## Próximo passo

O modelo pode ser convertido diretamente para DBML no arquivo da pasta `DBML`, gerando o diagrama e, depois, o SQL do SGBD escolhido.
