# ⭐ Star Schema para análise de Professores — Power BI

Projeto desenvolvido para a **Formação Power BI Analyst da DIO**, com foco na criação de um **modelo dimensional em Star Schema** a partir do diagrama relacional de uma universidade.

> **Escopo do desafio:** o objeto de análise é **Professor**. O modelo considera professores, departamentos, cursos, disciplinas e uma dimensão de datas. Dados de alunos e matrículas não fazem parte do escopo solicitado.

## 🎯 Objetivo

Transformar a visão relacional disponibilizada no desafio em um modelo dimensional simples e analítico, com uma tabela fato central e dimensões ao redor.

A granularidade escolhida foi:

> **1 linha da FatoProfessor = 1 oferta de uma disciplina ministrada por um professor, associada a um curso, departamento e data de oferta.**

## 🧱 Modelo dimensional

![Star Schema](docs/star-schema.png)

### Tabela fato

**FatoProfessor**

- `FatoProfessorKey` — Surrogate Key da ocorrência
- `ProfessorKey` — FK para DimProfessor
- `DepartamentoKey` — FK para DimDepartamento
- `CursoKey` — FK para DimCurso
- `DisciplinaKey` — FK para DimDisciplina
- `DataKey` — FK para DimData
- `QuantidadeOfertas` — medida aditiva com valor 1 por ocorrência

### Tabelas dimensão

**DimProfessor**
- `ProfessorKey` — Surrogate Key
- `idProfessor` — chave natural do modelo de origem
- `CodigoProfessor`

**DimDepartamento**
- `DepartamentoKey` — Surrogate Key
- `idDepartamento`
- `Nome`
- `Campus`

**DimCurso**
- `CursoKey` — Surrogate Key
- `idCurso`
- `CodigoCurso`

**DimDisciplina**
- `DisciplinaKey` — Surrogate Key
- `idDisciplina`
- `CodigoDisciplina`

**DimData**
- `DataKey` — Surrogate Key no formato `YYYYMMDD`
- `Data`
- `Ano`
- `Semestre`
- `Trimestre`
- `MesNumero`
- `MesNome`
- `AnoMes`

## 🔗 Relacionamentos

Todos os relacionamentos são do tipo **1 : muitos (*)**, com a dimensão no lado 1 e a fato no lado muitos:

- `DimProfessor[ProfessorKey]` → `FatoProfessor[ProfessorKey]`
- `DimDepartamento[DepartamentoKey]` → `FatoProfessor[DepartamentoKey]`
- `DimCurso[CursoKey]` → `FatoProfessor[CursoKey]`
- `DimDisciplina[DisciplinaKey]` → `FatoProfessor[DisciplinaKey]`
- `DimData[DataKey]` → `FatoProfessor[DataKey]`

## 🗓️ Dimensão de datas

O modelo relacional de referência não apresenta informações de data de oferta. Por isso, este projeto inclui uma **DimData demonstrativa**, assumindo datas de oferta de disciplinas/cursos para viabilizar análises temporais, conforme permitido no enunciado do desafio.

## 🚫 O que ficou fora do modelo

As entidades relacionadas a **Aluno**, **Matriculado** e **Pré-requisitos** não foram levadas para o Star Schema porque não são necessárias para o foco analítico definido: professores.

## 📁 Arquivos do repositório

```text
desafio-star-schema-professores/
├── README.md
├── dados/
│   ├── modelo_star_schema_professores.xlsx
│   ├── FatoProfessor.csv
│   ├── DimProfessor.csv
│   ├── DimDepartamento.csv
│   ├── DimCurso.csv
│   ├── DimDisciplina.csv
│   └── DimData.csv
├── docs/
│   ├── star-schema.png
│   ├── star-schema.svg
│   └── dicionario_dados.md
├── powerbi/
│   └── INSTRUCOES_POWER_BI.md
└── sql/
    └── star_schema.sql
```

## ⚡ Como abrir no Power BI Desktop

O arquivo `dados/modelo_star_schema_professores.xlsx` já contém todas as tabelas necessárias.

1. Abra o Power BI Desktop.
2. Vá em **Obter dados > Excel**.
3. Selecione `modelo_star_schema_professores.xlsx`.
4. Carregue as tabelas `FatoProfessor`, `DimProfessor`, `DimDepartamento`, `DimCurso`, `DimDisciplina` e `DimData`.
5. Na **Exibição de Modelo**, crie os cinco relacionamentos 1:* descritos acima.
6. Posicione a `FatoProfessor` no centro e as dimensões ao redor.
7. Marque `DimData` como tabela de datas usando a coluna `Data`.
8. Salve o arquivo como `desafio-star-schema-professores.pbix`.

Há um passo a passo detalhado em [`powerbi/INSTRUCOES_POWER_BI.md`](powerbi/INSTRUCOES_POWER_BI.md).

## 💡 Decisões de modelagem

- Uso de **Surrogate Key** nas dimensões para desacoplar o modelo analítico das chaves naturais do sistema transacional.
- Uso de uma **factless fact table** com `QuantidadeOfertas = 1`, permitindo contagens por professor, curso, disciplina, departamento e período.
- Dimensões desnormalizadas e ligadas diretamente à fato, mantendo o desenho de **Star Schema**.
- Exclusão de entidades fora do escopo analítico solicitado.

## ⚠️ Observação sobre os dados

Os registros presentes nas planilhas e CSVs são **dados fictícios demonstrativos** criados apenas para materializar a estrutura dimensional do desafio. O foco do projeto é a **modelagem**, não a reprodução de dados reais da universidade.

## 👨‍💻 Autor

Projeto desenvolvido por **Bruno Daniel** como parte da Formação Power BI Analyst da DIO.
