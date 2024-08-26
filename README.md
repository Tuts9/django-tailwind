# Instalação do Tailwind no Projeto Django

Documentação original [Django Tailwind]('https://django-tailwind.readthedocs.io/en/latest/installation.html')

## PIP

- Existe uma biblioteca do Python que facilita a integração do Tailwind nos projetos Django
    ```bash
    pip install django-tailwind
    ```
- Também é necessário instalar uma biblioteca para recarregamento automático da página em desenvolvimento
    ```bash
    pip install django-browser-reload
    ```

## Configuração

- Adicionar `'tailwind'` em `INSTALLED_APPS` no arquivo `settings.py`
    ```py
    INSTALLED_APPS = [
        # other Django apps
        'tailwind', # Adicione ao código
    ]
    ```

## Configuração da app `theme`

- Hora de criar a app Tailwind CSS para carregar todos os arquivos, normalmente chamada de `theme`
    ```bash
    python manage.py tailwind init
    ```

- Depois é preciso adicionar `'theme'` em `INSTALLED_APPS` no arquivo `settings.py`
    ```py
    INSTALLED_APPS = [
        # other Django apps
        'tailwind',
        'theme', # Adicione ao código
    ]
    ```

- Deve ser registrado o app `'theme'` gerado adicionando a seguinte linha no final do arquivo `settings.py`
    ```py
    TAILWIND_APP_NAME = 'theme'
    ```

- Certifique de que a lista `INTERNAL_IPS` esteja presente no arquivo `settings.py` e contenha o endereço `IP 127.0.0.1`
    ```py
    INTERNAL_IPS = [
        "127.0.0.1",
    ]
    ```

- OBSERVAÇÃO: Às vezes (especialmente no Windows), o executável Python não consegue encontrar o binário do `Node.js` instalado em seu sistema. <br> Neste caso, você precisa definir o caminho para o executável em `settings.py` manualmente.
  - Como alternativa (e para máxima confiabilidade), você pode usar um caminho totalmente qualificado. Pode ser assim:
  ```py
  NPM_BIN_PATH = r"C:\Program Files\nodejs\npm.cmd"
  ```

## Instalação das dependências

- Instale as dependências CSS do Tailwind, executando o seguinte comando:
    ```bash
    python manage.py tailwind install
    ```

## Adicionando nos templates

- No arquivo `base.html` podemos adicionar as seguintes tags:
    ```html
    {% load static tailwind_tags %}
    ...
    <head>
        ...
        {% tailwind_css %}
        ...
    </head>
    ```

## Finalização da configuração do `django-browser-reload`

- Vamos também adicionar e configurar django-browse-reload, que cuida das atualizações automáticas de página e css no modo de desenvolvimento. <br> Adicione-o a `INSTALLED_APPS` em `settings.py`:
    ```py
    INSTALLED_APPS = [
        # other Django apps
        'tailwind',
        'theme',
        'django_browser_reload', # Adicione isso no código
    ]
    ```

- Permanecendo em `settings.py`, adicione o middleware:
    ```py
    MIDDLEWARE = [
        # ...
        "django_browser_reload.middleware.BrowserReloadMiddleware", # Adicione no código
        # ...
    ]
    ```

- Inclua `django-browse-reload` URL em sua raiz `urls.py`:
    ```py
    from django.urls import include, path
        urlpatterns = [
            ...,
            path("__reload__/", include("django_browser_reload.urls")), # Adicione esta linha no seu código
        ]
    ```

## Finalização

- Finalmente já deve ser capaz de usar as classes CSS do Tailwind no projeto Django.
- Inicie o ambiente de desenvolvimento no terminal:
    ```bash
    python manage.py tailwind start
    ```

- Lembrando que para acompanhar as modificações sempre em tempo real é recomendado deixar sempre esse terminal
rodando e rodar o servidor em uma aba alternativa do terminal, se não será necessário ficar sempre
parando o servidor para rodar o comando `python manage.py tailwind build`

