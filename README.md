<div align="center">
 <img src="https://raw.githubusercontent.com/rex41788-arch/drive-editor-/refs/heads/main/banner.png" alt="Drive Editor" width="600" />
</div>

<h1 align="center">Drive Editor</h1>

<p align="center">
  <strong>Biblioteca Python para interagir com o Google Drive via links de compartilhamento</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/versão-1.0.0-blue?style=for-the-badge&logo=python&logoColor=white" alt="versão" />
  <img src="https://img.shields.io/badge/licença-MIT-22863a?style=for-the-badge" alt="licença" />
  <img src="https://img.shields.io/badge/python-3.6%2B-f6c90e?style=for-the-badge&logo=python&logoColor=black" alt="python" />
  <img src="https://img.shields.io/badge/dependências-requests-orange?style=for-the-badge" alt="dependências" />
</p>

<p align="center">
  <a href="mailto:rexturbo702@gmail.com">Feito por Rex</a>
</p>

---

## Índice

- [Sobre o Projeto](#sobre-o-projeto)
- [Instalação](#instalação)
- [Pré-requisitos](#pré-requisitos)
- [Funcionalidades Detalhadas](#funcionalidades-detalhadas)
- [Exemplos Rápidos](#exemplos-rápidos)
- [Guia de Uso Completo](#guia-de-uso-completo)
- [Permissões e Limitações](#permissões-e-limitações)
- [Solução de Problemas Comuns](#solução-de-problemas-comuns)
- [Contribuição](#contribuição)
- [Licença](#licença)

---

## Sobre o Projeto

**Drive Editor** é uma biblioteca Python desenvolvida para simplificar a interação com o Google Drive. Em vez de enfrentar a complexidade do OAuth, gerenciamento de tokens e configurações demoradas, você só precisa de **links de compartilhamento**.

A biblioteca se conecta a um backend seguro que já possui uma conta Google pré-autorizada. Você fornece o link da pasta (com permissão de **editor**) e a biblioteca cuida de todo o resto: upload, download, exclusão, movimentação e renomeação de arquivos e pastas.

**Ideal para:**
- Automações de backup e sincronização
- Scripts pessoais de gerenciamento de arquivos
- Integrações rápidas com o Google Drive em aplicações Python
- Qualquer projeto que precise interagir com o Drive sem complicações

---

## Instalação

A instalação é feita via pip com um único comando:

```bash
pip install drive-editor
```

> **Nota:** A única dependência externa é a biblioteca `requests`, que é instalada automaticamente.

---

## Pré-requisitos

- **Python 3.6 ou superior**
- **Conexão com a internet** (para comunicação com o backend)
- **Links de compartilhamento do Google Drive** com permissão de **editor** para as pastas/arquivos que deseja manipular

---

## Funcionalidades Detalhadas

| Operação | Método | Descrição |
|:---:|:---|:---|
| **Upload** | `drive.upload()` | Envia arquivos locais para pastas compartilhadas. O link da pasta é obrigatório. |
| **Listagem** | `drive.listar()` | Lista todos os arquivos e subpastas de uma pasta. Também exige o link da pasta. |
| **Download** | `drive.download()` | Baixa arquivos individuais ou pastas inteiras (como `.zip`). Para pastas, o nome do arquivo baixado será o nome da pasta com extensão `.zip`. |
| **Exclusão** | `drive.excluir()` | Remove permanentemente arquivos ou pastas. |
| **Movimentação** | `drive.mover()` | Move arquivos ou pastas de uma pasta para outra. |
| **Renomeação** | `drive.renomear()` | Altera o nome de um arquivo ou pasta. |

> **Importante:** Os métodos `upload()` e `listar()` **exigem** o parâmetro `pasta_link`. Se você não fornecer um link, a biblioteca lançará um erro `ValueError`. Isso foi projetado intencionalmente para evitar que arquivos sejam acidentalmente enviados para a raiz da conta autorizada.

---

## Exemplos Rápidos

Aqui estão alguns exemplos práticos para você começar rapidamente:

```python
from drive_editor import DriveEditor

drive = DriveEditor()

# Upload de uma imagem para uma pasta compartilhada
drive.upload("foto.jpg", "https://drive.google.com/drive/folders/PASTA_ID")

# Listar arquivos de uma pasta
arquivos = drive.listar("https://drive.google.com/drive/folders/PASTA_ID")
for arq in arquivos:
    tipo = "pasta" if arq.get('mimeType') == 'application/vnd.google-apps.folder' else "arquivo"
    print(f"{tipo}: {arq['name']} (ID: {arq['id']})")

# Baixar um arquivo específico
drive.download("https://drive.google.com/file/d/ARQUIVO_ID/view", "meu_arquivo.pdf")

# Excluir um arquivo
drive.excluir("https://drive.google.com/file/d/ARQUIVO_ID/view")
```

---

## Guia de Uso Completo

### 1. Inicialização

```python
from drive_editor import DriveEditor

drive = DriveEditor()
```

### 2. Upload de Arquivos

```python
# Upload simples
drive.upload("foto.jpg", "https://drive.google.com/drive/folders/PASTA_ID")

# Upload com substituição automática (se o arquivo já existir)
drive.upload("relatorio.docx", "https://drive.google.com/drive/folders/PASTA_ID", substituir=True)
```

### 3. Listar Arquivos de uma Pasta

```python
arquivos = drive.listar("https://drive.google.com/drive/folders/PASTA_ID")
for arq in arquivos:
    tipo = "pasta" if arq.get('mimeType') == 'application/vnd.google-apps.folder' else "arquivo"
    print(f"{tipo}: {arq['name']} (ID: {arq['id']})")
```

### 4. Download

```python
# Download de um arquivo individual
drive.download("https://drive.google.com/file/d/ARQUIVO_ID/view", "meu_arquivo.pdf")

# Download de uma pasta inteira (gerado como .zip)
drive.download("https://drive.google.com/drive/folders/PASTA_ID", "pasta.zip")
```

### 5. Excluir Arquivos ou Pastas

```python
drive.excluir("https://drive.google.com/file/d/ARQUIVO_ID/view")
```

### 6. Mover Arquivos ou Pastas

```python
drive.mover(
    "https://drive.google.com/file/d/ARQUIVO_ID/view",
    "https://drive.google.com/drive/folders/NOVA_PASTA"
)
```

### 7. Renomear Arquivos ou Pastas

```python
resultado = drive.renomear(
    "https://drive.google.com/file/d/ARQUIVO_ID/view",
    "novo_nome.txt"
)
print(resultado)  # Exibe o ID e o novo nome
```

---

## Permissões e Limitações

A conta Google utilizada pelo backend precisa ter permissão de **editor** sobre os itens que você deseja modificar. Abaixo está um resumo do que é permitido:

| Ação | Permitido |
|:---|:---:|
| Adicionar, remover e modificar arquivos dentro de pastas compartilhadas | ✔️ |
| Renomear arquivos e subpastas | ✔️ |
| Excluir ou mover a pasta raiz compartilhada | ❌️ |

**Explicação:** A permissão de editor concede controle sobre o **conteúdo** da pasta, mas não sobre a pasta raiz em si. Para gerenciar a pasta raiz (excluí-la ou movê-la), você precisa ser o **proprietário** dela.

---

## Solução de Problemas Comuns

### Erro: "ValueError: É obrigatório fornecer o link da pasta compartilhada"

**Causa:** Você tentou fazer upload ou listagem sem fornecer o link da pasta.  
**Solução:** Sempre inclua o parâmetro `pasta_link` com um link válido.

### Erro: "HttpError 403 - The user does not have sufficient permissions"

**Causa:** A conta autorizada não tem permissão de editor no item que você está tentando modificar.  
**Solução:** Compartilhe a pasta ou arquivo com o e-mail da conta autorizada (rexturbo702@gmail.com) com permissão de **editor**.

### Erro: "Erro na requisição: 404 Client Error"

**Causa:** O link fornecido é inválido ou o backend está fora do ar.  
**Solução:** Verifique o link e certifique-se de que o backend está acessível (acesse `https://drive-backend-api.onrender.com/health` para testar).

### O download de pastas gera um arquivo com nome genérico

**Causa:** O nome original da pasta não pôde ser obtido.  
**Solução:** Certifique-se de que a pasta existe e que você tem permissão de leitura. O backend tentará usar o nome da pasta; se falhar, o arquivo será salvo como `pasta.zip`.

---

## Contribuição

Contribuições são muito bem-vindas! Siga os passos abaixo para configurar o ambiente de desenvolvimento:

```bash
# Clone o repositório
git clone https://github.com/rexturbo/drive-editor.git
cd drive-editor

# Instale em modo editável
pip install -e .
```

### Como Contribuir

1. Faça um **fork** do projeto.
2. Crie uma branch para sua feature: `git checkout -b feature/nova-feature`.
3. Commit suas alterações: `git commit -m 'feat: adiciona nova feature'`.
4. Envie para o repositório: `git push origin feature/nova-feature`.
5. Abra um **Pull Request**.

### Relatando Problemas

Se encontrar um bug ou tiver uma sugestão, [abra uma issue](https://github.com/rexturbo/drive-editor/issues) com:
- Descrição clara do problema
- Passos para reproduzir
- Logs ou screenshots relevantes

---

## Licença

Distribuído sob a licença **MIT**. Consulte o arquivo [LICENSE](https://github.com/rex41788-arch/drive-editor-/blob/main/LICENSE) para mais detalhes.

---

<p align="center">
  Desenvolvido por <a href="mailto:rexturbo702@gmail.com"><strong>Rex</strong></a>
  <br>
  <sub>Se este projeto foi útil para você, considere deixar uma estrela no GitHub!</sub>
</p>
