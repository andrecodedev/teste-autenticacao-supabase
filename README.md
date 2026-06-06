# Teste de Autenticação — Caixa Cinza com Supabase e Postman

## Introdução

Este repositório contém a documentação completa da atividade de testes de caixa cinza aplicados a uma API de autenticação. Os testes foram realizados utilizando o Supabase como backend de autenticação e o Postman como ferramenta de execução das requisições HTTP.

Testes de caixa cinza são testes em que o testador possui conhecimento parcial da estrutura interna do sistema, como endpoints, headers, formato do body e respostas esperadas, sem ter acesso ao código-fonte completo.

---

## Objetivo da Atividade

- Configurar um ambiente de autenticação utilizando o Supabase
- Criar usuários de teste
- Compreender o funcionamento de requisições HTTP
- Utilizar o Postman para execução de testes
- Validar respostas de autenticação
- Documentar tecnicamente todo o processo realizado

---

## Configuração do Supabase

### Como o projeto foi criado

O projeto foi criado na plataforma [Supabase](https://supabase.com) com as seguintes configurações:

- **Nome do projeto:** `teste-autenticacao`
- **Organização:** `andrecodedev`
- **Região:** Americas (East US - North Virginia)
- **Plano:** Free

### Tipo de autenticação utilizada

Foi utilizada autenticação por **e-mail e senha** (Email Provider), que é o método padrão do Supabase. Esse método permite que usuários se autentiquem fornecendo um endereço de e-mail e uma senha previamente cadastrados.

### Como o usuário foi criado

O usuário foi criado manualmente pelo painel do Supabase em **Authentication → Users → Add user → Create new user**, com as seguintes credenciais:

- **Email:** `teste@email.com`
- **Password:** `Senha@123`
- A opção **Auto confirm user** foi marcada, dispensando a necessidade de confirmação por e-mail.

### Objetivo da configuração

A configuração do Supabase serve como backend de autenticação da aplicação. Ele fornece uma API REST que permite autenticar usuários via HTTP, retornando tokens de acesso (JWT) que podem ser utilizados para autorizar requisições futuras.

### Credenciais da API

| Variável | Valor |
|---|---|
| Project URL | `https://hmuslbaxymwzqecakdrc.supabase.co` |
| API Key (anon) | `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...` |

---

## Configuração do Postman

### Como o Workspace foi criado

Foi utilizado o Workspace padrão **My Workspace** do Postman Desktop. Dentro dele foi criada a collection **Testes Autenticacao Supabase** para organizar todas as requisições da atividade.

### Como o Environment foi configurado

Foi criado um Environment chamado **Supabase - Testes** com as seguintes variáveis:

| Variável | Descrição |
|---|---|
| `base_url` | URL base da API do Supabase |
| `api_key` | Chave anon pública da API |
| `email` | E-mail do usuário de teste |
| `password` | Senha do usuário de teste |

### Finalidade das variáveis

O uso de variáveis de ambiente permite reutilizar as credenciais em múltiplas requisições sem repetição, facilitando a manutenção e tornando os testes mais organizados e seguros.

### Como o Postman foi utilizado nos testes

O Postman foi utilizado para criar e executar requisições HTTP POST contra o endpoint de autenticação do Supabase, simulando diferentes cenários de login para validar o comportamento da API.

---

## Configuração das Requisições

### Endpoint utilizado

```
POST {{base_url}}/auth/v1/token?grant_type=password
```

### Método HTTP

`POST`

### Headers utilizados

| Header | Valor |
|---|---|
| `apikey` | `{{api_key}}` |
| `Authorization` | `Bearer {{api_key}}` |
| `Content-Type` | `application/json` |

### Body JSON utilizado

```json
{
  "email": "usuario@email.com",
  "password": "senha123"
}
```

### Finalidade da requisição

Esta requisição realiza a autenticação de um usuário via e-mail e senha. Em caso de sucesso, a API retorna um `access_token` (JWT) que pode ser utilizado para autorizar requisições subsequentes.

---

## Execução dos Testes

### Cenários executados

Foram executados 5 cenários de teste conforme especificado na atividade:

1. **Login válido** — credenciais corretas
2. **Senha incorreta** — e-mail válido com senha errada
3. **Usuário inexistente** — e-mail não cadastrado
4. **Campos vazios** — e-mail e senha em branco
5. **Credenciais inválidas** — e-mail mal formatado

### Tabela de Cenários

| Cenário | Entrada Utilizada | Resultado Esperado | Resultado Obtido | Status |
|---|---|---|---|---|
| Login válido | `teste@email.com` / `Senha@123` | Status 200 + access_token | 200 OK + token retornado | ✅ OK |
| Senha incorreta | `teste@email.com` / `senhaerrada` | Erro de autenticação | 400 — invalid_credentials | ✅ OK |
| Usuário inexistente | `naoexiste@email.com` / `Senha@123` | Acesso negado | 400 — invalid_credentials | ✅ OK |
| Campos vazios | `""` / `""` | Erro de validação | 400 — validation_failed / missing email or phone | ✅ OK |
| Credenciais inválidas | `emailinvalido` / `qualquercoisa` | Mensagem de erro | 400 — invalid_credentials | ✅ OK |

---

## Registro dos Testes

### Como os testes foram registrados

Os testes foram registrados em uma planilha contendo ID, cenário, entrada, resultado esperado, resultado obtido, status e observações. A planilha está disponível na pasta `/planilha` deste repositório.

### Importância da documentação dos testes

A documentação dos testes é fundamental para garantir rastreabilidade, facilitar a identificação de falhas, e servir como evidência do processo de qualidade realizado. Em projetos reais, essa documentação é exigida por auditorias e processos de qualidade de software.

### Falhas identificadas

Nenhuma falha crítica foi identificada. O sistema respondeu corretamente em todos os cenários, retornando os status HTTP e mensagens de erro adequados.

---

## Resultados Obtidos

| Cenário | Status HTTP | Resposta |
|---|---|---|
| Login válido | `200 OK` | `access_token`, `refresh_token`, dados do usuário |
| Senha incorreta | `400 Bad Request` | `"error_code": "invalid_credentials"` |
| Usuário inexistente | `400 Bad Request` | `"error_code": "invalid_credentials"` |
| Campos vazios | `400 Bad Request` | `"error_code": "validation_failed"` |
| Credenciais inválidas | `400 Bad Request` | `"error_code": "invalid_credentials"` |

---

## Conclusão

### Os testes foram executados corretamente?

Sim. Todos os 5 cenários obrigatórios foram executados com sucesso no Postman Desktop, com evidências registradas em prints.

### A autenticação funcionou?

Sim. O endpoint `/auth/v1/token?grant_type=password` do Supabase respondeu corretamente em todos os cenários, retornando um `access_token` válido no caso de login bem-sucedido e mensagens de erro adequadas nos demais casos.

### Dificuldades encontradas

- A instalação do Postman no Linux (Debian) via snap apresentou erro de compatibilidade de versão, sendo necessário instalar manualmente via arquivo `.tar.gz`.
- O environment do Postman precisou ser selecionado manualmente para que as variáveis `{{base_url}}` e `{{api_key}}` fossem resolvidas corretamente.

### Falhas identificadas

Nenhuma falha no comportamento da API foi identificada. O sistema se comportou conforme o esperado em todos os cenários testados.

### Melhorias que poderiam ser implementadas

- Adicionar testes automatizados com scripts no Postman (aba Scripts) para validar automaticamente o status HTTP e o conteúdo da resposta.
- Implementar rate limiting para evitar ataques de força bruta na autenticação.
- Adicionar testes com tokens expirados e refresh token.

### Importância dos testes caixa cinza em APIs

Os testes de caixa cinza são fundamentais em APIs de autenticação pois permitem validar o comportamento do sistema com conhecimento parcial de sua estrutura, simulando cenários reais de uso e abuso. Eles identificam vulnerabilidades e comportamentos inesperados sem exigir acesso ao código-fonte, sendo amplamente utilizados em processos de QA e segurança de software.
