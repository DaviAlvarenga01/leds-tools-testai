
# Test.AI - Guia de Uso

## ✅ Como Rodar o Test.AI

### 🔧 Pré-requisitos

Antes de começar, verifique se você tem os seguintes softwares instalados:

- [Python](https://www.python.org/) (incluindo o gerenciador de pacotes `pip`)
- [Visual Studio Code](https://code.visualstudio.com/)
- [Node.js](https://nodejs.org/) (recomendado)

---

### 📦 PASSO 1: Instalar a Biblioteca Python

1. Abra um terminal.
2. Execute o comando:

```bash
pip install test-ai-leds
```

3. Certifique-se de que o diretório `Scripts` do Python está adicionado à variável de ambiente `PATH`.

#### 🔹 Windows

- Execute:

```bash
pip show test-ai-leds
```

- Será exibido um caminho semelhante a:

```
C:\Users\user\AppData\Local\Packages\PythonSoftwareFoundation.Python.3.12_qbz5n2kfra8p0\LocalCache\local-packages\Python312\site-packages
```

- Substitua `site-packages` por `Scripts`, por exemplo:

```
C:\Users\user\AppData\Local\Packages\PythonSoftwareFoundation.Python.3.12_qbz5n2kfra8p0\LocalCache\local-packages\Python312\Scripts
```

- Adicione esse caminho nas variáveis de ambiente do sistema.

> 💡 Você também pode usar **virtual environments** para evitar instalação global.

#### 🔹 Linux

- Crie um ambiente virtual:

```bash
python -m venv <nome-da-venv>
```

- Ative a venv:

```bash
source <caminho-da-venv>/bin/activate
```

- Instale a biblioteca:

```bash
pip install test-ai-leds
```

- Adicione o caminho dos scripts ao `.bashrc`:

```bash
nano ~/.bashrc
```

Adicione ao final do arquivo:

```bash
export PATH=$PATH:/<caminho-da-venv>/bin
```

- Depois, execute:

```bash
source ~/.bashrc
```

---

### 🧩 PASSO 2: Instalar a Extensão Test.AI no VS Code

1. Abra o **Visual Studio Code**.
2. Acesse a aba de extensões (ícone de quadrado ou atalho `Ctrl + Shift + X`).
3. Busque por `Test.AI`.
4. Instale a extensão.

---

### ⚙️ PASSO 3: Configurar o Arquivo `.env`

Na pasta raiz do repositório (`leds-tools-testai`), crie um arquivo chamado `.env` com o seguinte conteúdo:

```env
LLM_MODEL=gemini/gemini-1.5-flash
GEMINI_API_KEY=<Sua Chave>
SWAGGER_PATH=<Caminho para o Swagger>
DTO_SOURCE=<Caminho para a pasta DTO>
```

#### Exemplo:

```env
LLM_MODEL=gemini/gemini-1.5-flash
GEMINI_API_KEY=asduf24385HDSuyad43trfjedsig
SWAGGER_PATH=C:/Users/usuario/OneDrive/Documentos/PS2/leds-tools-testai/dtos/swagger.json
DTO_SOURCE=C:/Users/usuario/OneDrive/Documentos/PS2/leds-tools-testai/dtos
```

---

## ⚡ Funcionalidades

### ✅ Funcionalidade 1: Gerar Arquivos de Código Gherkin (Features, BDD)

#### 📌 Pré-requisitos

- Um arquivo `.andes` dentro da pasta `andes` que está dentro da pasta (`leds-tools-testai`).

#### ▶️ Como executar

No terminal, dentro do repositório (`leds-tools-testai`), rode:

```bash
python src/application/use_cases/crew_gherkin.py
```

- Digite o nome do arquivo `.andes` (sem a extensão).
- O arquivo `.feature` será gerado automaticamente na pasta `features` com o nome `resposta.feature`.

---

### ✅ Funcionalidade 2: Gerar Steps das Features (C# com xUnit)

#### 📌 Pré-requisitos

- Um arquivo `.feature` dentro da pasta `features`que está dentro da pasta (`leds-tools-testai`).

#### ▶️ Como executar

No terminal, dentro do repositório (`leds-tools-testai`), rode:

```bash
python src/application/use_cases/crew_xUnit.py
```

- Digite o nome do arquivo `.feature` (sem a extensão).
- O arquivo `resposta.cs` será gerado na pasta `resposta` que está dentro da pasta (`leds-tools-testai`).

---



## 🏛️ Arquitetura Adotada
Estilo Arquitetural: Clean Architecture

Clean Architecture é uma estrutura de design de software com várias camadas, promovendo uma estrutura organizada e de fácil compreensão, o que é benéfico para o desenvolvimento.

Sua principal característica é a separação e independência das camadas, como o desacoplamento da lógica de negócios do sistema de influências externas como o sistema de interface do usuário (UI), frameworks, bancos de dados e assim por diante. Isso é alcançado definindo uma camada de domínio independente e isolada.

🧠 Princípios Fundamentais da Clean Architecture:
- Independência de tecnologia: O núcleo do sistema (regras de negócio) não conhece detalhes de frameworks, bibliotecas ou I/O.

- Ordem de dependência: O fluxo de dependência sempre aponta para o centro — interfaces externas dependem do domínio, e nunca o contrário.

- Regras de negócio isoladas: Permite reaproveitamento em outros contextos (ex: CLI, APIs, interfaces gráficas).

A principal ideia da Clean Architecture é separar o código em camadas concêntricas, onde:

🔄 As dependências sempre apontam para dentro:

```

+------------------------+
| External Layer | <- Interface com o usuário, web, banco, etc.
+------------------------+
| Interface Adapters | <- Controllers, Gateways, Presenters
+------------------------+
| Use Cases Layer | <- Regras de negócio da aplicação
+------------------------+
| Entities (Core) | <- Regras de negócio mais genéricas
+------------------------+
```


🧱 As camadas:
- Frameworks & Drivers (camada externa):
Onde ficam os frameworks, banco de dados, UI, serviços externos, etc.

- Interface Adapters:
Camada que adapta os dados para entrada/saída (ex: controllers, presenters, repositórios).

- Use Cases:
Casos de uso da aplicação, orquestram as regras para resolver problemas específicos do domínio.

- Entities:
Contém as regras de negócio mais genéricas e independentes de tecnologia.

## 🧩 Aplicação no contexto do Test.AI:

| Camada | Descrição | Exemplos |
|----------|----------|----------|
| Interface  | Camada de interação com o usuário. Usa a API do VS Code para capturar ações como clique direito.  | Comandos como "Generate BDD", "Generate Steps"  |
| Aplicação  | Camada que define os fluxos principais e regras de orquestração dos dados.  | Script que decide como gerar arquivos a partir dos dados fornecidos.  |
| Domínio  | Contém as regras de negócio puras, como interpretação do .andes e geração dos testes.  | Classes e funções Python que fazem parsing e estruturam os dados.  |
| Infraestrutura  | Responsável por interagir com o sistema operacional, arquivos, APIs externas, .env.  | Integração com Gemini, leitura de .env, gravação de arquivos.  |

## 🖥️ Interface no Test.AI

### 📌 Descrição
A **camada de Interface** é responsável por interagir diretamente com o **usuário final**. No projeto Test.AI, essa interação é realizada por meio de uma aplicação em **Streamlit**, que serve como a camada de apresentação visual, exibindo os dados gerados pelas APIs e capturando comandos do usuário.

Essa camada não processa lógica de negócio, mas atua como ponte entre o usuário e a aplicação.

---

### 🔍 Trechos do Código Relacionados à Interface

**Arquivo:** `src/scripts/comparacao.py`

| Trecho de Código | Descrição | Função na Interface |
|------------------|-----------|---------------------|
| `import streamlit as st` | Importa a biblioteca de interface gráfica. | Inicializa a camada de interface web. |
| `user_input = data_json['payload']` | Carrega a entrada do usuário a partir de um JSON. | Captura o dado a ser enviado para as APIs. |
| `if st.button("Enviar"):` | Cria o botão de envio. | Dispara o processamento ao clicar. |
| `col1, col2 = st.columns([1, 1])` | Cria duas colunas visuais na interface. | Divide as respostas do modelo "Debate" e "Sequencial". |
| `st.session_state['messages'].append(...)` | Armazena as mensagens da sessão. | Controla o histórico da interação. |
| `st.write(...)` | Exibe as respostas e entrada do usuário. | Mostra os dados na tela para o usuário. |
| `if st.button("Limpar Conversa"):` | Cria botão de limpeza da sessão. | Reseta a interface e o histórico da conversa. |

---

### ✅ Resumo

A camada de Interface no Test.AI atua como um **painel de controle visual** da aplicação. Ela é responsável por:

- **Receber dados de entrada do usuário**
- **Enviar esses dados para APIs externas (Debate e Sequencial)**
- **Exibir os resultados recebidos de forma clara e organizada**
- **Manter o histórico da sessão de forma interativa**
- **Resetar a interface sob demanda**

## 🖥️ Aplicação no Test.AI

### 📌 Descrição
A **camada de Aplicação** A camada de aplicação pode ser considerada o epicentro do projeto. Como o seu nome sugere, é nesse estado onde o aplicativo é desenvolvido, onde trazemos o funcionamento à lógica definida anteriormente na camada de domínio. 
Nessa camada se encontra a materealização e a execução dos casos de uso, desta forma, definindo o comportamento do aplicativo.

---

### ✅ Portanto..

A camada de Aplicação no Test.AI atua como um o **cerne** da aplicação. Ela é responsável por:

- **Executar os casos de uso de geração de BDD**
- **Controla a interação entre os agentes e os fluxos de ação**
- **Isola a lógica de negócio da interface**
- **Guarda a definição dos agentes**
- **Guarda os arquivos referentes a definição das tarefas**

## 🖥️ Domínio no Test.AI

### 📌 Descrição

A **camada de Domínio da Lógica** no **Test.AI** é responsável por conter as **regras de negócio** para o funcionamento de um sistema. Fundamental para garantir que todas as ações de interpretação de dados e geração de arquivos de testes sejam feitas de forma correta e eficiente.

Essa camada é **independente de frameworks e tecnologias externas**, isolando a lógica de negócio central do resto do sistema, o que facilita testes, manutenção e modificações sem impactar outras partes do projeto.

---

### 🔍 **Como funciona**

1. **Inicialização de Modelos LLM**:
   - Usa duas configurações de LLM (temperaturas diferentes): uma mais criativa (`temp=0.6`) e outra mais precisa (default, `temp=0.0`).
   - As funções `init_llm`, `init_agent` e `init_task` são importadas de `module.py`.

2. **Ciclo de Geração em Rodadas**:
   - Roda três iterações para gerar e revisar arquivos `.feature`.
   - Em cada rodada:
     - Um agente “**gherkin_writer**” escreve o código Gherkin.
     - Um agente “**gherkin_reviewer**” revisa o código gerado.
     - Ambos são configurados dinamicamente com base no turno (ex: `rodada_1`, `rodada_2`, etc.).

3. **Definição de Tarefas**:
   - Tarefas são criadas a partir de descrições dinâmicas (`tasks_dict`), onde o `user_case` é inserido no texto da tarefa para contextualização.

4. **Uso de uma Estrutura de "Crew"**:
   - Ao que tudo indica (com base no nome das classes `Agent`, `Task`, `Crew`), está utilizando a biblioteca `crewai`, que estrutura o uso de múltiplos agentes colaborativos.

#### 🧠 Inteligência Artificial

A integração com LLM permite que esses agentes gerem conteúdo mais natural, completo e alinhado com os padrões do Gherkin, mesmo com uma entrada simples como um caso de uso (`user_case`).

---

### ✅ Resumo

Esse script representa um componente da **Camada de Domínio** que automatiza a **criação colaborativa de testes BDD** com uso de inteligência artificial e múltiplos agentes especializados. É aqui que reside a lógica principal para **converter requisitos textuais em testes executáveis** com validação automatizada.


## 🖥️ Infraestrutura no Test.AI

### 📌 Descrição

Este módulo define a infraestrutura backend do Test.AI utilizando o FastAPI como framework principal, com suporte a CORS, tratamento de requisições REST e integração com modelos de linguagem (LLMs) através da biblioteca crewai. O foco principal é o recebimento de eventos via POST, que são processados por agentes inteligentes para gerar arquivos de especificação de testes em formato Gherkin (BDD). Os resultados são orquestrados, revisados e consolidados por múltiplos agentes para produzir um artefato final de teste.

***CORS é um mecanismo usado para adicionar cabeçalhos HTTP que informam aos navegadores para permitir que uma aplicação Web seja executada em uma origem e acesse recursos de outra origem diferente.***

---

### 🔍 Trechos do Código Relacionados à Interface

**Arquivo:** `src/app/main.py`

```
crew = Crew(
agents=agents + [manager],
tasks=tasks + [final_task],
max_rpm=10,
output_log_file="crew_log.txt",
manager_llm=llm_low_temp,
process=Process.sequential,
verbose=True
)
```
- Este trecho define a Crew com múltiplos agentes (writers, reviewers e manager) e suas respectivas tarefas. O processo é executado de forma sequencial, e os logs são salvos em crew_log.txt. A CrewAI orquestra toda a execução das tarefas com uso de LLMs configurados dinamicamente.

```
load_dotenv()
```

- Carrega as variáveis de ambiente a partir de um arquivo .env. Isso é essencial para o funcionamento correto da aplicação, especialmente para o uso de chaves de API como a GOOGLE_API_KEY necessária para configurar os LLMs utilizados pelos agentes da CrewAI.

```
@app.get("/")
async def home():
return "Rodando"
```

- Este endpoint básico verifica se a aplicação está no ar.

```
@app.post("/gherkin")
async def generate_gherkin_file(evento: Evento):
feature = generate_gherkin_feature(evento.evento)
body = {
"feature": feature
}
return JSONResponse(body)
```


- O endpoint /gherkin é o principal ponto de entrada para a geração de arquivos de teste. Ele recebe um JSON com um campo evento, que será transformado em uma feature Gherkin através da função generate_gherkin_feature.

- A função generate_gherkin_feature cria múltiplos agentes (writers e revisores), cada um com funções específicas na construção e verificação de cenários de teste. Um agente gerente sintetiza os melhores resultados em um único arquivo .feature.

---

### ✅ Resumo

- Backend criado com FastAPI, com suporte a CORS.

- Utilização da biblioteca CrewAI para orquestrar agentes inteligentes baseados em LLMs (modelo Gemini via GOOGLE_API_KEY).

- Entrada via POST em /gherkin recebe eventos e os transforma em cenários BDD (Gherkin).

- O processo de geração envolve múltiplos agentes:

- Escritores e revisores de cenários Gherkin.

- Um gerente que unifica as versões geradas.

- Resultado final é salvo em arquivo .feature e registrado em log (crew_log.txt).

## 🌐 Tecnologias no Front-end vs Back-end
|Camada	| Tecnologia	| Descrição |
|----------|----------|----------|
|Front-end	| VS Code Extension (TypeScript) |	Responsável pela interação com o usuário e acionamento dos comandos.
|Back-end	| Python (test-ai-leds)	| Responsável por processar os dados, interpretar arquivos e gerar os códigos.
|Back-end   | Crew.ai | Plataforma de multi-agentes, os quais fazem a automação dos fluxos de testes.|
|Back-end   | LLM-model | Utiliza o modelo Gemini 1.5 flash como backend da LLM |


## 📚 Referências Bibliográficas

1. https://medium.com/@gabrielfernandeslemos/clean-architecture-uma-abordagem-baseada-em-princípios-bf9866da1f9c

2. https://www.alura.com.br/artigos/como-resolver-erro-de-cross-origin-resource-sharing?srsltid=AfmBOorV-xhK1EvpyB2zY9hm9hDnIj3HivlXWoIbFQJZE9jbESQLfXbC

3. https://requests.readthedocs.io/en/latest/

4. https://docs.streamlit.io

5. https://github.com/leds-org/leds-tools-public/blob/main/docs/test_ai/img/fluxograma_gherkin.png

## ⚠️ Observação

**Pasta ```scripts``` contém código para testar as entidades task e agent que foram refatoradas**

