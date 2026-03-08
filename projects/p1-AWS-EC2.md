# Avaliação 01 – Instalação do Ollama + Open WebUI na EC2

## Objetivo

Configurar uma instância na **AWS EC2** e instalar o **Ollama** juntamente com o **Open WebUI** para executar modelos de inteligência artificial localmente e acessá-los via interface web.

---

# 1. Criar a Instância na AWS

Acesse a plataforma da AWS e abra o serviço **EC2**.

### Configurações da instância

**Nome**

```
ollama-server
```

**Sistema Operacional (AMI)**

```
Ubuntu Server 22.04 LTS
```

**Tipo da Instância**

```
t3a.large
```

**Armazenamento**

```
30 GB
```

**Key Pair**
Criar ou selecionar uma chave SSH para acessar a instância.

Exemplo:

```
ollama-key.pem
```

---

# 2. Configurar Security Group

Adicionar regras de entrada (**Inbound Rules**):

| Tipo       | Porta | Origem    |
| ---------- | ----- | --------- |
| SSH        | 22    | 0.0.0.0/0 |
| HTTP       | 80    | 0.0.0.0/0 |
| HTTPS      | 443   | 0.0.0.0/0 |
| Custom TCP | 8080  | 0.0.0.0/0 |

A porta **8080** será utilizada pelo Open WebUI.

Salvar as regras.

---

# 3. Conectar na Instância EC2

No terminal do seu computador:

```
chmod 400 ollama-key.pem
```

Conectar via SSH:

```
ssh -i ollama-key.pem ubuntu@IP_DA_INSTANCIA
```

---

# 4. Atualizar o Sistema

Dentro da instância execute:

```
sudo apt update
sudo apt upgrade -y
```

---

# 5. Instalar o Ollama

Executar o script de instalação:

```
curl -fsSL https://ollama.com/install.sh | sh
```

---

# 6. Verificar Instalação do Ollama

Verificar o status do serviço:

```
systemctl status ollama
```

Se não estiver ativo:

```
sudo systemctl start ollama
```

Ativar inicialização automática:

```
sudo systemctl enable ollama
```

---

# 7. Baixar um Modelo de IA

Executar um modelo no Ollama:

```
ollama run mistral
```

ou

```
ollama run llama3
```

Durante a primeira execução o modelo será **baixado automaticamente**.

Após finalizar o download, pressione:

```
CTRL + C
```

---

# 8. Verificar Modelos Instalados

Listar modelos disponíveis:

```
ollama list
```

Exemplo de saída:

```
NAME            SIZE
mistral:latest  4.1GB
llama3:latest   4.7GB
```

---

# 9. Instalar o Open WebUI

Instalar o gerenciador snap:

```
sudo apt install snapd -y
```

Instalar o Open WebUI:

```
sudo snap install open-webui --beta
```

---

# 10. Executar o Open WebUI

Iniciar o serviço:

```
open-webui
```

Saída esperada:

```
open-webui.listener: active
http://0.0.0.0:8080
```

---

# 11. Acessar a Interface Web

Abrir no navegador:

```
http://IP_PUBLICO_DA_EC2:8080
```

Exemplo:

```
http://54.210.34.19:8080
```

Isso abrirá a interface gráfica do **Open WebUI**.

---

# 12. Criar um Modelo no Open WebUI

Na interface web:

1. Clique em **Create Model**
2. Preencha:

**Name**

```
ChronosIA
```

**Model Tag Name**

```
chronosia:latest
```

**ModelFile**

```
FROM mistral

SYSTEM You are an AI assistant specialized in programming and data analysis.
```

Clique em **Save & Create**.

---

# 13. Testar o Modelo

Após criar o modelo:

1. Abra um novo chat
2. Selecione o modelo criado
3. Envie uma pergunta

Exemplo:

```
Explain what artificial intelligence is.
```

Se o modelo responder, o sistema está funcionando corretamente.

---

# Conclusão

Após os passos acima, foi possível:

* Criar uma instância EC2 na AWS
* Instalar o Ollama
* Baixar modelos de inteligência artificial
* Instalar o Open WebUI
* Acessar uma interface web para interação com os modelos

Essa configuração permite executar **modelos de IA localmente na instância da AWS**, sem depender diretamente de APIs externas.

---
