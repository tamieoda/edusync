[LEIA-ME.md](https://github.com/user-attachments/files/27050950/LEIA-ME.md)
# Sistema de Gerenciamento — Aulas de Inglês

Aplicação **single-file** (apenas `index.html`) para gerenciar alunos, aulas, pagamentos e despesas, com sincronização opcional no Google Drive.

---

## 🚀 Como começar (modo rápido — só no navegador)

1. Abra `index.html` no navegador (duplo clique).
2. Tudo já funciona: os dados ficam salvos no `localStorage` do navegador.
3. Clique em **+ Novo Aluno** e comece a cadastrar.

> ⚠️ No modo "abrir direto do arquivo" (`file://`) o Google Drive **não funciona** — é uma limitação do Google, não do app. Use a seção abaixo para ativar a sincronização.

---

## ☁️ Ativando a sincronização com o Google Drive

Você precisa hospedar o `index.html` em uma URL real (http/https) e criar credenciais OAuth.

### Parte 1 — Hospedar o arquivo

Escolha uma opção:

**Opção A — GitHub Pages (grátis e mais usado)**
1. Crie conta no [github.com](https://github.com).
2. Crie um repositório chamado `seuusuario.github.io` (público).
3. Faça upload do `index.html`.
4. Acesse `https://seuusuario.github.io` — pronto.

**Opção B — Servidor local (só no seu computador)**
Abra o terminal na pasta e rode:
```
python3 -m http.server 8000
```
Acesse `http://localhost:8000` no navegador.

### Parte 2 — Criar credenciais OAuth

1. Acesse [console.cloud.google.com](https://console.cloud.google.com/).
2. Crie um novo projeto (qualquer nome, ex.: "Aulas de Inglês").
3. No menu lateral: **APIs & Services → Library** → pesquise "Google Drive API" → clique em **Enable**.
4. Volte para **APIs & Services → OAuth consent screen**:
   - User Type: **External** → Create.
   - Preencha nome do app, email de suporte, email do desenvolvedor.
   - Clique **Save and Continue** nas próximas telas.
   - Na seção **Test users**, adicione seu email.
5. Vá em **APIs & Services → Credentials → Create Credentials → OAuth client ID**:
   - Application type: **Web application**.
   - **Authorized JavaScript origins**: adicione a URL onde você hospedou (ex.: `https://seuusuario.github.io` **ou** `http://localhost:8000`).
   - Clique em **Create**.
6. Copie o **Client ID** que aparece (formato: `xxxxxxx.apps.googleusercontent.com`).

### Parte 3 — Conectar o app

1. Abra a aplicação no navegador.
2. Vá na aba **Configurações**.
3. Cole o Client ID no campo correspondente → **Salvar Configurações**.
4. No topo da página, clique em **Conectar Drive**.
5. Autorize na janela do Google.
6. Pronto: a cada alteração o app salva automaticamente no seu Google Drive no arquivo `gerenciamento-alunos.json`.

---

## 📋 Funcionalidades

### Aba Alunos
- Cadastro completo: nome, endereço, contato, NIF (contribuinte), email, data de nascimento, início do curso, mensalidade.
- **Aulas**: adicione data + matéria dada. Saldo calculado automaticamente.
- **Pagamentos mensais**: data, mês de referência, valor, checkbox de recibo emitido.
- Estatísticas ao vivo: meses pagos, aulas que têm direito (8 por mês), aulas feitas, saldo.
- **Histórico completo** por aluno: botão abre modal com todas as aulas agrupadas por mês.

### Aba Despesas
- Registre despesas por mês: Aluguel, Luz, Contabilista, Segurança Social, Seguro, Material, Outro.
- Total do mês calculado automaticamente.

### Aba Relatório Mensal
- Número de alunos, valor recebido, despesas, saldo.
- **Saldo negativo destacado em vermelho com sinal `-`**.
- Alerta de recibos pendentes (alunos que pagaram mas não emitiram recibo).
- Alerta de alunos sem pagamento registrado no mês.
- Detalhamento de aulas e saldo por aluno.
- **Exportar PDF** do relatório (útil para o contabilista).

### Aba Configurações
- Google Drive (Client ID).
- Exportar/importar JSON de backup.
- Apagar todos os dados (com dupla confirmação).

---

## 🔐 Sobre privacidade

- Tudo fica **no seu Google Drive** (no app que você mesmo criou). A Anthropic e nenhum terceiro têm acesso.
- O escopo `drive.file` só permite ao app ler/editar arquivos que **ele próprio criou** — não tem acesso ao resto do seu Drive.
- Os dados também ficam salvos no `localStorage` do navegador como backup local.

---

## 🛠 Estrutura de pastas

```
Sistema de gerenciamento/
├── index.html       # A aplicação inteira (abra no navegador)
└── LEIA-ME.md       # Este arquivo
```

---

## 🐛 Bug do cursor (resolvido)

A implementação anterior tinha um problema em que o cursor sumia a cada caractere digitado. Neste sistema:

- **Nunca recriamos inputs enquanto você digita.** Cada input tem identificadores (`data-sid`, `data-field`) e um listener global atualiza o estado sem tocar no DOM do input.
- Só re-renderizamos quando você **adiciona ou remove** uma linha (aula, pagamento, despesa, aluno).
- Resultado: você pode digitar livremente em qualquer campo, o cursor fica onde está.
