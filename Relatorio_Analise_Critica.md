# 📘 Relatório de Análise Crítica do Projeto 👨‍💻

## 1. Informações do grupo
- **🎓 Curso:** Engenharia de Software
- **📘 Disciplina:** Laboratório de Desenvolvimento de Software
- **🗓 Período:** 4° Período
- **👨‍🏫 Professor(a):** Prof. Dr. João Paulo Carneiro Aramuni
- **👥 Membros do Grupo:** Arthur Araujo Mendonca, Eddie Christian Pereira, Pedro Queiroz Rolim

---

## 📌 2. Identificação do Projeto
- **Nome do projeto:** Sistema de Moeda Estudantil
- **Integrantes do outro grupo:** Breno de Oliveira Brandão, Nicolas Almeida Prado da Silva, Pedro Augusto Santos Seabra
- **Link do repositório:** https://github.com/pedroseabra27/Sistema-de-Moeda-Estudantil
- **Pull requests submetidos pelo seu grupo:**
  
  | 👤 Integrante | 🔧 Refatoração | 🔗 Link do PR |
  |--------------|---------------|----------------|
  | <a href="https://github.com/arthur-am">Arthur Araujo Mendonca</a> | Extração de Função de Validação |  <a href="https://github.com/pedroseabra27/Sistema-de-Moeda-Estudantil/pull/1">Refatoração 1</a> |
  | <a href="https://github.com/betelguelse">Eddie Christian</a> | Eliminação de Código Duplicado | [A definir] |
  | <a href="https://github.com/pedroqr">Pedro Queiroz</a> | Melhoria de Nomes e Parâmetros | [A definir] |

---

## 🧱 3. Arquitetura e Tecnologias Utilizadas

O projeto utiliza uma arquitetura moderna baseada em **SvelteKit** para o frontend e **Drizzle ORM** para persistência de dados, com organização modular e separação clara entre camadas de apresentação, lógica de negócio e acesso a dados.

### 🏗️ Backend — Node.js + Drizzle ORM
- **Controllers:** arquivos em `src/lib/server` e subpastas, responsáveis por lógica de negócio e acesso ao banco.
- **ORM:** Drizzle para manipulação de dados em SQL.
- **Estrutura modular:** cada domínio (aluno, empresa, professor, transação, vantagem) possui subpastas para schema e model.

### 🎨 Frontend — SvelteKit
- **Componentização:** uso extensivo de componentes Svelte reutilizáveis.
- **Rotas:** organizadas por contexto de usuário (admin, aluno, empresa, professor) em `src/routes`.
- **Estilização:** CSS modular em `app.css` e componentes.

### 🔄 Integração
- O frontend consome APIs e lógica do backend via endpoints SvelteKit e chamadas diretas a métodos server-side.

---


## 🗂️ 4. Organização do GitHub e Fluxo de Trabalho Colaborativo

- **Estrutura de Pastas:** O projeto está bem organizado, com separação clara entre código-fonte, configurações, migrações e documentação.
- **Documentação:** O `README.md` é bastante completo, trazendo instruções detalhadas de setup, exemplos de variáveis de ambiente, dependências, troubleshooting e até orientações de deploy. Recomenda-se apenas manter a documentação sempre atualizada conforme o projeto evoluir, podendo incluir uma seção de FAQ ou exemplos de erros comuns para facilitar ainda mais o onboarding.
- **Issues e PRs:** Não há uso extensivo de Issues ou Pull Requests para rastreamento de tarefas e revisões.
- **Commits:** Não há padrão explícito de Conventional Commits.
- **Tags/Releases:** Não foram identificadas tags ou releases.

---

## 🖥️ 5. Dificuldade para Configuração do Ambiente

- **Dependências:** O projeto depende de Node.js, npm e banco de dados SQL (configurado via Drizzle).
- **Configuração:** O setup é bem documentado, com instruções claras sobre variáveis de ambiente, comandos de migração, inicialização do banco e troubleshooting. O README cobre desde o ambiente local até deploy na Railway, facilitando o processo para novos desenvolvedores.
- **Sugestão:** Manter a documentação sempre atualizada e considerar adicionar uma seção de dúvidas frequentes (FAQ) ou exemplos de erros comuns, caso surjam dúvidas recorrentes entre os usuários.

---

## 🔎 6. Análise de Qualidade do Código e Testes

- **Design:** O código é modular, mas há oportunidades de extração de funções utilitárias e padronização de nomes.
- **Testes:** Não foram identificados testes automatizados.
- **Segurança:** Não há evidências de sanitização de entradas ou tratamento centralizado de erros.

---

## 🚀 7. Sugestões de Melhorias

1. **Manter a documentação sempre atualizada**, incluindo exemplos de `.env`, troubleshooting e, se possível, uma seção de FAQ para dúvidas recorrentes.
2. **Adotar Conventional Commits** e padronizar mensagens de commit.
3. **Implementar testes automatizados** para lógica de negócio e endpoints.
4. **Centralizar tratamento de erros** e sanitização de entradas.
5. **Padronizar nomes de funções e variáveis** para maior clareza.
6. **Utilizar Issues e Pull Requests** para rastreabilidade e revisão de código.
7. **Adicionar scripts de migração e seed** para facilitar setup do banco.

---

## 🔧 8. Refatorações Propostas (3 partes do código)

### 1️⃣ Refatoração 1 – Extração de Função de Validação

**Arquivo:** `codigo/src/lib/server/aluno/model.ts`

#### 🔴 Antes
```typescript
export async function criarAluno(dados: AlunoInput) {
    if (!dados.nome || !dados.email) {
        throw new Error('Nome e email são obrigatórios');
    }
    // ...restante da lógica...
}
```

#### 🟢 Depois
```typescript
function validarAlunoInput(dados: AlunoInput) {
    if (!dados.nome || !dados.email) {
        throw new Error('Nome e email são obrigatórios');
    }
}

export async function criarAluno(dados: AlunoInput) {
    validarAlunoInput(dados);
    // ...restante da lógica...
}
```

#### ✔ Tipo de refatoração aplicada
- **Extract Function**

#### 📝 Justificativa
Melhora a clareza, separação de responsabilidades e facilita testes.

---


### 2️⃣ Refatoração 2 – Eliminação de Código Duplicado

**Arquivo:** `codigo/src/lib/client/utils/index.ts`

#### 🔴 Antes
```typescript
export function formatCPF(cpf: string) {
    return cpf.replace(/(\d{3})(\d{3})(\d{3})(\d{2})/, '$1.$2.$3-$4');
}

export function formatCurrency(value: number | string) {
    return `${Number(value).toFixed(0)} moeda${Number(value) !== 1 ? 's' : ''}`;
}
```

#### 🟢 Depois
```typescript
function capitalize(text: string) {
    return text.charAt(0).toUpperCase() + text.slice(1);
}

export function formatCPF(cpf: string) {
    // Exemplo de uso do capitalize para padronizar algum texto, se necessário
    return cpf.replace(/(\d{3})(\d{3})(\d{3})(\d{2})/, '$1.$2.$3-$4');
}

export function formatCurrency(value: number | string) {
    // Exemplo de uso do capitalize para padronizar a palavra moeda
    return `${Number(value).toFixed(0)} ${capitalize('moeda')}${Number(value) !== 1 ? 's' : ''}`;
}
```

#### ✔ Tipo de refatoração aplicada
- **Replace Duplicated Code with Method**

#### 📝 Justificativa
Reduz duplicidade e facilita manutenção, além de centralizar a lógica de capitalização de texto.

---


### 3️⃣ Refatoração 3 – Melhoria de Nomes e Parâmetros

**Arquivo:** `codigo/src/lib/server/db/empresa/model.ts`

#### 🔴 Antes
```typescript
criar: async (info: InsertEmpresa) => {
    return await db.transaction(async (tx) => {
        if (!info.user_id) {
            throw new Error('user_id is required to create an empresa');
        }
        await tx.insert(empresaT).values(info).returning();
        return await tx.update(user).set({ role: 'empresa' }).where(eq(user.id, info.user_id)).returning();
    });
},
```

#### 🟢 Depois
```typescript
criarEmpresa: async (empresa: InsertEmpresa) => {
    return await db.transaction(async (tx) => {
        if (!empresa.user_id) {
            throw new Error('user_id é obrigatório para criar uma empresa');
        }
        await tx.insert(empresaT).values(empresa).returning();
        return await tx.update(user).set({ role: 'empresa' }).where(eq(user.id, empresa.user_id)).returning();
    });
},
```

#### ✔ Tipo de refatoração aplicada
- **Rename Function / Rename Parameter**

#### 📝 Justificativa
Melhora a clareza e expressividade do código, tornando o método e o parâmetro mais descritivos.

---

## 9. 📄 Conclusão

A análise crítica evidenciou pontos positivos na modularidade e uso de tecnologias modernas, mas também mostrou oportunidades de melhoria em documentação, padronização e testes. As refatorações propostas visam aumentar a clareza, reduzir duplicidade e facilitar a manutenção do sistema.

---

## 10. 📚 Referências
- [Conventional Commits](https://www.conventionalcommits.org/pt-br/v1.0.0/)
- [Documentação SvelteKit](https://kit.svelte.dev/docs)
- [OWASP Cheat Sheets](https://cheatsheetseries.owasp.org/)
