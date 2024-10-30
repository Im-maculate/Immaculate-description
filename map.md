Criei uma organização chamada: https://github.com/Im-maculate. A partir dessa organização, dois repositórios foram inicializados im_core e immaculate

Criar uma camada de Decentralized Identity na blockchain da Hathor utilizando ZPK no repositório im_core, com:
	- com Python
	- .env
	- sem ambiente virtual
	- Preciso das credenciais e chaves de API da Hathor para criar essa segunda camada?

Criar uma intreface gráfica no repositório immaculate para essa camada utilizando:
	- Python e django ou Ruby on rails


Para criar uma camada de Decentralized Identity (DID) na blockchain da Hathor usando Zero-Knowledge Proofs (ZKP), e implementar uma interface gráfica com Python/Django ou Ruby on Rails, será necessário seguir algumas etapas organizadas. O projeto será dividido em duas partes: a implementação da lógica de DID no repositório `im_core` e a criação da interface gráfica no repositório `immaculate`.

## Parte 1: Criando a Camada de Decentralized Identity no repositório `im_core`

#### Estrutura do Repositório `im_core`

A estrutura de diretórios e arquivos deve ser a seguinte:
```markdown
im_core/
│
├── .env
├── app.py
├── requirements.txt
└── did_handler.py
```

#### 1. Criar o arquivo `.env`
O arquivo `.env` armazenará as credenciais e chaves de API da Hathor. Um exemplo de conteúdo:
```plaintext
HATHOR_API_URL=https://api.hathor.network -> Preciso confirmar esse e entender esse ponto
HATHOR_API_TOKEN=  -> Preciso confirmar esse e entender esse ponto
```

#### 2. Criar o arquivo `requirements.txt`
Neste arquivo, listar as dependências necessárias. Um exemplo:
```plaintext
requests
python-dotenv
```

#### 3. Criar o arquivo `did_handler.py`
Neste arquivo, a lógica para interagir com a Hathor e criar a camada de DID pode ser implementada. Um exemplo simplificado da implementação:
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

#### 4. Criar o arquivo `app.py`.
O arquivo principal que inicializa a aplicação e faz chamadas à `DIDHandler` deve conter o seguinte:
```python
from did_handler import DIDHandler

def main():
    did_handler = DIDHandler()

    did_data = {
        "name": "", # -> Preciso confirmar esse e entender esse ponto
        "description": "", # -> Preciso confirmar esse e entender esse ponto
        "public_key": "" # -> Preciso confirmar esse e entender esse ponto
    }

    created_did = did_handler.create_did(did_data)
    print("DID Created:", created_did)

    did_to_check = "did:example:123456789" # -> Preciso confirmar esse e entender esse ponto
    did_info = did_handler.verify_did(did_to_check)
    print("DID Info:", did_info)

if __name__ == "__main__":
    main()
```

#### 5. Executar a aplicação

Para instalar as dependências e executar o projeto, utilizar os seguintes comandos:

```bash
# Instalar as dependências
pip install -r requirements.txt
```
```bash
# Executar a aplicação
python app.py
```

## Parte 2: Criando a Interface Gráfica no repositório `immaculate`

Agora, deve-se estruturar a aplicação Django ou Ruby on Rails para criar a interface gráfica.

#### Estrutura do Repositório `immaculate`

Para uma aplicação Django, a estrutura deve ser a seguinte:
```markdown
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

#### 6. Criar o arquivo `requirements.txt`

Neste arquivo, adicionar as dependências necessárias para o Django:

```plaintext
Django
requests
python-dotenv
```

#### 7. Criar a aplicação Django

Executar os seguintes comandos para criar a aplicação Django:

```bash
# Criar um projeto Django
django-admin startproject django_app
```
```bash
# Navegar para o diretório do projeto
cd django_app
```
```bash
# Criar uma aplicação
python manage.py startapp app
```

#### 8. Configurar `settings.py`

No arquivo `settings.py`, adicionar a nova aplicação e configurar o template:

```python
INSTALLED_APPS = [
    ...
    'app',
]

TEMPLATES = [
    {
        ...
        'DIRS': [os.path.join(BASE_DIR, 'templates')],
        ...
    },
]
```

#### 9. Criar o arquivo `views.py`

Adicionar a lógica para manipular requisições:

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

#### 10. Criar o arquivo `urls.py`

Configurar as URLs da aplicação:
```python
from django.urls import path
from .views import index, create_did

urlpatterns = [
    path('', index, name='index'),
    path('create-did/', create_did, name='create_did'),
]
```

#### 11. Criar o arquivo `index.html`

Criar a interface para criar um DID:
```python
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

#### 12. Executar a Aplicação Django

Para iniciar o servidor, utilizar os seguintes comandos:

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

#### *1. Hathor Core:* Será necessário utilizar o `hathor-core` para interagir mais diretamente com a blockchain?

#### *2. Credenciais:* As credenciais e chaves de API podem ser obtidas ao criar uma conta no Hathor e solicitar acesso à API.

#### *3. ZKP:* Para implementar ZKP, pode-se usar bibliotecas como `PyCryptodome` ou outras bibliotecas específicas de ZKP conforme necessário.
