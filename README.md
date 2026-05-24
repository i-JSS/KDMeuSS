<div align="center">

# KD MEU SS?

![Java](https://img.shields.io/badge/Java_20-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot_3.1-6DB33F?style=for-the-badge&logo=springboot&logoColor=white)
![Maven](https://img.shields.io/badge/Maven-C71A36?style=for-the-badge&logo=apachemaven&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)
![Status](https://img.shields.io/badge/Status-Work_in_Progress-yellow?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

---

<img src="Images/PaginaInicial.png" style="width: 70%">
<img src="Images/Grade.png" style="width: 70%">

</div>

---

## Sobre o projeto

**KD MEU SS?** é um gerador automático de grade horária para alunos da UnB. O sistema recebe as matérias que o aluno deseja cursar, aplica um algoritmo de detecção de conflitos e devolve todas as combinações de turmas possíveis sem sobreposição de horários — incluindo preferência de professor.

O back-end é uma API REST construída com Spring Boot. O front-end é uma página estática em HTML, CSS e TypeScript que consome essa API diretamente.

---

## ⚙Como funciona

O coração do projeto é o `GerenciadorGrades`, que realiza três etapas:

**1. Filtragem de preferências**
Para cada matéria escolhida pelo aluno, o sistema busca na memória todas as turmas disponíveis com aquele nome. Turmas com o mesmo código de horário são agrupadas e seus professores concatenados, permitindo que o aluno veja todas as opções de professor para aquele slot.

**2. Geração de combinações**
A partir das turmas filtradas, o algoritmo percorre recursivamente todas as combinações possíveis (produto cartesiano das turmas por disciplina), seguindo o princípio do "tracinho" — para cada matéria, uma turma é escolhida e combinada com as demais.

**3. Verificação de conflitos**
Cada combinação gerada passa pela verificação de conflitos antes de ser incluída no resultado. O algoritmo compara todos os pares de matérias da combinação e verifica:

- Se as matérias são do mesmo **turno** (Manhã, Tarde, Noite)
- Se há sobreposição de **dias** da semana
- Se os intervalos de **horas** se sobrepõem

Somente combinações sem nenhum conflito são retornadas ao front-end.

---

## API — Endpoints

Base URL: `http://localhost:8080`

| Método | Rota | Descrição |
|--------|------|-----------|
| `POST` | `/gradesFiltradas/preferencias` | Recebe a lista de matérias escolhidas e retorna todas as grades possíveis sem conflito |
| `GET`  | `/gradesFiltradas/listaMaterias` | Retorna os nomes de todas as matérias disponíveis |
| `GET`  | `/gradesFiltradas/listaProfessores` | Retorna os nomes de todos os professores disponíveis |

### Exemplo de requisição — `POST /gradesFiltradas/preferencias`

```json
[
  {
    "nome": "Cálculo 1",
    "professor": "",
    "codigo": ""
  },
  {
    "nome": "Introdução à Ciência da Computação",
    "professor": "",
    "codigo": ""
  }
]
```

### Exemplo de resposta

```json
[
  {
    "materias": [
      {
        "nome": "Cálculo 1",
        "professor": "Prof. Silva",
        "codigo": "MAT001A",
        "horario": [{ "turno": "M", "dia": [2, 4], "hora": [1, 2] }]
      },
      {
        "nome": "Introdução à Ciência da Computação",
        "professor": "Prof. Souza",
        "codigo": "CIC001B",
        "horario": [{ "turno": "M", "dia": [3, 5], "hora": [3, 4] }]
      }
    ]
  }
]
```

---

## Como rodar

### Pré-requisitos

- Java 20+
- Maven

### Back-end

```bash
# Clone o repositório
git clone https://github.com/i-JSS/KDMeuSS.git
cd KDMeuSS

# Suba a API
./mvnw spring-boot:run
```

A API ficará disponível em `http://localhost:8080`.

### Front-end

Abra o arquivo `src/main/resources/static/index.html` diretamente no navegador, ou sirva a pasta `static` com qualquer servidor HTTP local (ex: Live Server do VS Code). O front já aponta para `localhost:8080` por padrão.

---

## Estrutura do projeto

```
KDMeuSS/
├── src/
│   └── main/
│       ├── java/com/joaoseisei/KDMeuSS/
│       │   ├── KDMeuSSApplication.java      # Entry point
│       │   ├── Controle.java                # REST Controller
│       │   ├── GerenciadorGrades.java       # Algoritmo de grades e conflitos
│       │   ├── Dados.java                   # Dados das matérias em memória
│       │   └── Model/
│       │       ├── Materia.java
│       │       ├── Horario.java
│       │       └── Grade.java
│       └── resources/
│           └── static/                      # Front-end (HTML/CSS/TS)
├── Images/                                  # Screenshots
├── pom.xml
└── README.md
```
