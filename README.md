# Descrição do Projeto Immaculate

O projeto **"Immaculate"** visa desenvolver uma aplicação que permite a verificação da identidade de uma pessoa (por exemplo, CPF) utilizando **Zero-Knowledge Proofs (ZKP)** e **Decentralized Identifiers (DIDs)**. O sistema será leve, escalável e seguirá os padrões W3C para DIDs. A aplicação será dividida em duas partes principais: a lógica de DIDs e a interface gráfica.

## MAPPING do Projeto

### M: Missão
- Criar uma solução que utilize DIDs e ZKPs para validar a identidade de forma segura e privada.

### A: Arquitetura
- O projeto terá dois repositórios:
  1. **im_core**: Lógica de DIDs e integração com a blockchain da Hathor.
  2. **immaculate**: Interface gráfica desenvolvida em Django ou Ruby on Rails.

### P: Processo
- **Parte 1**: Implementar a camada de Decentralized Identity no repositório **im_core**.
- **Parte 2**: Criar a interface gráfica no repositório **immaculate**.

### P: Produtos
- **im_core**: Biblioteca Python para gestão de DIDs.
- **immaculate**: Aplicação web para interação do usuário com a funcionalidade de DIDs.

### I: Interação
- A interface gráfica permitirá que os usuários criem e verifiquem DIDs através de formulários, interagindo com a API da Hathor.

### N: Necessidades
- Credenciais e chaves de API da Hathor para autenticar requisições.
- Instalação do Python e bibliotecas necessárias (requests, python-dotenv, Django).

### G: Governança
- O projeto será mantido em repositórios no GitHub, seguindo boas práticas de versionamento e documentação.


## Estrutura de Códigos

### 1. Repositório im_core

**Estrutura do Diretório:**
```bash
im_core/
│
├── .env
├── app.py
├── requirements.txt
└── did_handler.py
```

**Código do arquivo .env:**
```plaintext
HATHOR_API_URL=
HATHOR_API_TOKEN=
```

**Código do arquivo requirements.txt:**
```plaintext
requests
python-dotenv
```

**Código do arquivo did_handler.py:**
```python
import os
import requests
from dotenv import load_dotenv

load_dotenv()

class DIDHandler:
    def __init__(self):
        self.api_url = os.getenv("HATHOR_API_URL")
        self.api_token = os.getenv("HATHOR_API_TOKEN")
    
    def create_did(self, did_data):
        headers = {
            'Authorization': f'Token {self.api_token}',
            'Content-Type': 'application/json'
        }
        
        response = requests.post(f'{self.api_url}/dids', json=did_data, headers=headers)
        return response.json()

    def verify_did(self, did):
        headers = {
            'Authorization': f'Token {self.api_token}',
        }

        response = requests.get(f'{self.api_url}/dids/{did}', headers=headers)
        return response.json()
```

**Código do arquivo app.py:**
```python
from did_handler import DIDHandler

def main():
    did_handler = DIDHandler()

    did_data = {
        "name": "Nome do Usuário",  #-> Nome de exemplo
        "description": "Descrição do Usuário", #-> Descrição de exemplo
        "public_key": "Chave Pública" #-> Chave Pública de exemplo
    }

    created_did = did_handler.create_did(did_data)
    print("DID Created:", created_did)

    did_to_check = "did:example:123456789" #-> DID de exemplo
    did_info = did_handler.verify_did(did_to_check)
    print("DID Info:", did_info)

if __name__ == "__main__":
    main()
```

**Execução da Aplicação:**
```bash
# Instalar as dependências
pip install -r requirements.tx
```

```bash
# Executar a aplicação
python app.py
```
------------------------------------------------------

### 2. Repositório immaculate
**Estrutura do Diretório:**

```plaintext
immaculate/
│
├── manage.py
├── requirements.txt
└── django_app/
    ├── __init__.py
    ├── settings.py
    ├── urls.py
    ├── views.py
    ├── templates/
    │   └── index.html
```

**Código do arquivo requirements.txt:**
```bash
Django
requests
python-dotenv
```

**Código do arquivo views.py:**
```python
from django.shortcuts import render
from django.http import JsonResponse
import requests
import os
from dotenv import load_dotenv

load_dotenv()

def index(request):
    return render(request, 'index.html')

def create_did(request):
    if request.method == 'POST':
        did_data = {
            "name": request.POST.get('name'),
            "description": request.POST.get('description'),
            "public_key": request.POST.get('public_key'),
        }
        
        api_url = os.getenv("HATHOR_API_URL")
        api_token = os.getenv("HATHOR_API_TOKEN")

        headers = {
            'Authorization': f'Token {api_token}',
            'Content-Type': 'application/json'
        }

        response = requests.post(f'{api_url}/dids', json=did_data, headers=headers)
        return JsonResponse(response.json())
```

**Código do arquivo urls.py:**
```python
from django.urls import path
from .views import index, create_did

urlpatterns = [
    path('', index, name='index'),
    path('create-did/', create_did, name='create_did'),
]
```

**Código do arquivo index.html:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Create DID</title>
</head>
<body>
    <h1>Create a New DID</h1>
    <form method="POST" action="/create-did/">
        {% csrf_token %}
        <input type="text" name="name" placeholder="DID Name" required>
        <input type="text" name="description" placeholder="DID Description" required>
        <input type="text" name="public_key" placeholder="Public Key" required>
        <button type="submit">Create DID</button>
    </form>
</body>
</html>
```

**Execução da Aplicação Django:**
```bash
# Navegar para o diretório do projeto
cd django_app
```
```bash
# Instalar as dependências
pip install -r requirements.txt
```
```bash
# Executar o servidor
python manage.py runserver
```

## Considerações Finais

- **Hathor Core:** Verificar se a utilização do **hathor-core** é necessária para interações mais avançadas com a blockchain.
  
- **Credenciais:** As credenciais e chaves de API devem ser obtidas ao criar uma conta na Hathor e solicitar acesso à API.
  
- **Zero-Knowledge Proofs:** Pesquisar sobre bibliotecas que suportem a implementação de ZKPs.
