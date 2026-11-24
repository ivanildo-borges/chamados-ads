# Chamados ADS

- Por: Cicero Ivanildo Borges Alves

## Objetivo

API REST desenvolvida em Python com Flask para a disciplina de Programação para Web. O projeto está na etapa inicial: por enquanto, a API oferece uma rota de verificação de funcionamento; o cadastro, a consulta e a atualização de chamados com persistência serão implementados nas próximas etapas.

## Sobre o projeto

Uma equipe de TI recebe solicitações como “impressora sem conexão” e “computador não inicia”. Cada chamado precisa de identificação e descrição, e a equipe deve poder consultar os registros e atualizar o andamento. Esses dados devem continuar disponíveis após reiniciar o programa, e diferentes clientes devem poder usar o mesmo serviço. O serviço recebe essas operações e responde a elas; inicialmente, um cliente HTTP como o Postman será usado para interagir com a API.

## Tecnologias

- **Python 3.14 ou superior**: linguagem do projeto, conforme `pyproject.toml`.
- **Flask**: framework que fornece as rotas HTTP e as respostas da API.
- **uv**: gerenciador de projeto e dependências Python. Usa `pyproject.toml` para declarar dependências e `uv.lock` para registrar versões reproduzíveis. Ao contrário de manter apenas um `requirements.txt`, também pode criar e sincronizar o ambiente virtual do projeto.

## Requisitos

- Python 3.14 ou superior.
- [uv](https://docs.astral.sh/uv/getting-started/installation/) para a instalação recomendada.

## Instalação e execução com uv

No terminal, a partir da pasta do projeto:

```bash
uv sync --locked
uv run flask run
```

`uv sync` instala as dependências declaradas no projeto e cria ou atualiza o ambiente virtual local (`.venv`), respeitando o arquivo de lock. `uv run` executa o Flask nesse ambiente. O servidor de desenvolvimento ficará disponível, por padrão, em `http://127.0.0.1:5000`.

Para iniciar o servidor com recarga automática durante o desenvolvimento:

```bash
uv run flask run --debug
```

## Instalação e execução sem uv

Com Python 3.14 ou superior instalado, crie e ative um ambiente virtual e instale o projeto:

```bash
python -m venv .venv
```

No PowerShell:

```powershell
.\.venv\Scripts\Activate.ps1
python -m pip install -e .
flask run
```

No macOS ou Linux:

```bash
source .venv/bin/activate
python -m pip install -e .
flask run
```

O Flask detecta o `app.py` na pasta atual. Também é possível indicar explicitamente a aplicação com `flask --app app run`. Mantenha o ambiente virtual ativado ao executar `flask run`.

## Usando a API

Com o servidor em execução, envie pelo Postman uma requisição:

```http
GET http://127.0.0.1:5000/
```

Resposta atual:

```json
{
  "mensagem": "API de chamados em funcionamento"
}
```

Essa rota serve apenas para confirmar que a aplicação está ativa. Ainda não existem rotas para criar, consultar ou atualizar chamados, nem armazenamento persistente; portanto, essas funcionalidades descritas no objetivo do projeto não estão implementadas nesta versão.

## Estrutura do projeto

```text
chamados-ads/
├── .gitignore
├── .python-version
├── app.py
├── LICENSE
├── pyproject.toml
├── README.md
├── uv.lock
└── src/
    └── chamados_ads/
        └── __init__.py
```

Arquivos e diretórios gerados localmente, como `.venv/`, `__pycache__/`, configurações privadas `.env.*` e bancos de dados locais, não devem ser versionados. O arquivo `.env.example` pode ser mantido no repositório como modelo sem segredos.

## Para que serve cada arquivo

- **`.gitignore`**: exclui do Git ambientes virtuais, arquivos temporários, caches e dados locais.
- **`.python-version`**: indica a versão de Python usada pelo projeto e reconhecida pelo uv.
- **`app.py`**: cria a aplicação Flask e define a rota HTTP inicial.
- **`LICENSE`**: contém os termos de licença do projeto.
- **`pyproject.toml`**: identifica o projeto, declara a versão mínima de Python, as dependências e a configuração de empacotamento.
- **`README.md`**: apresenta o projeto e explica instalação, execução e uso.
- **`uv.lock`**: fixa as versões resolvidas das dependências para instalações consistentes; deve ser versionado junto com `pyproject.toml`.
- **`src/chamados_ads/__init__.py`**: inicializa o pacote Python e contém o ponto de entrada de linha de comando configurado no projeto. Esse comando é separado do servidor Flask.

## Como funciona a aplicação Flask

Em `app.py`, `from flask import Flask, jsonify` importa a classe da aplicação e a função auxiliar `jsonify`. `app = Flask(__name__)` cria a instância que o comando `flask run` localiza.

O decorator `@app.get("/")` associa a função `inicio()` à rota `/` para requisições HTTP **GET**. Quando um cliente acessa essa rota, a função retorna um dicionário Python com `mensagem`. O Flask converte automaticamente esse dicionário em uma resposta JSON.

Uma API REST pode usar outros verbos HTTP conforme as operações: por exemplo, `POST` para criar um chamado, `GET` para consultar, e `PATCH` ou `PUT` para atualizar. Esses endpoints ainda não fazem parte da implementação atual.
