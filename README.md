![](https://i.imgur.com/IpfYxQ2.png)

Há PWA (Progressive Web App), aplicativos nativos para Android, IOS e via Terminal. Então desenvolvi o projeto feito em `python` usando duas bibliotecas. `pywebview` e `pyinstaller`. A primeira biblioteca faz com que use o usuário use website dentro de uma aplicação customizável. Isto é, você mantém o site original para visualizar apenas. Já a segunda biblioteca compila um programa em python em executável independente do sistema operacional usado.

#

Código fonte:

```py
import webview

class Api:
    def __init__(self):
        webview.settings['ALLOW_DOWNLOADS'] = True
        webview.settings['OPEN_EXTERNAL_LINKS_IN_BROWSER'] = False
        webview.settings['OPEN_DEVTOOLS_IN_DEBUG'] = False

    def read_cookies(self, window):
        cookies = window.get_cookies()
        for c in cookies:
            print(c.output())

    def clear_cookies(self, window):
        window.clear_cookies()
        print('Cookies cleared')

if __name__ == '__main__':
    api = Api()

    window = webview.create_window(
        'TabNews',
        'https://tabnews.com.br',
        js_api=api,
        confirm_close=True
    )

    webview.start(api.read_cookies, window, user_agent='Chrome/51.0.2704.103', private_mode=False)
```
> Segue a documentação para baixar o `pywebview`: https://pywebview.flowrl.com/

Vamos explicar o que eu fiz até aqui. Acompanhe:

Em caso de salvar algum arquivo pelo TabNews, será perguntado ao seu sistema onde salvar, do contrário, seria salvo sem seu consentimento.
```py
webview.settings['ALLOW_DOWNLOADS'] = True
```
Aqui permite você acessar quaisquer link dentro da aplicação, do contrário, ira-se abrir o seu navegador a cada link acessado.
```py
webview.settings['OPEN_EXTERNAL_LINKS_IN_BROWSER'] = False
```
Para um leigo é suficiente o acesso à página. Então bloqueei o DevToos, por segurança. 
```py
webview.settings['OPEN_DEVTOOLS_IN_DEBUG'] = False
```
Essa parte, está relacionado aos cookies, simulando como navegador tradicional. Do contrário, a escolha do usuário para tema "dark" ou crendenciais não seriam salvos. Também não é bom manter todo o tempo os cookies, por segurança ou perfomance.
```py
def read_cookies(self, window):
        cookies = window.get_cookies()
        for c in cookies:
            print(c.output())

    def clear_cookies(self, window):
        window.clear_cookies()
        print('Cookies cleared')
```
Por fim, eu estou simulando um "user_agent", não se faz nercessário neste caso. Porque ele funciona normal. Etretanto, por prevenção, quis inserir por compatibilidade.
```py
user_agent='Chrome/51.0.2704.103',
```
O restante do trecho não explicado, já se percebe à lógica. Como `'https://tabnews.com.br'`, ou seja, destino que quero visualizar e seja carregado o site para mim. Exclusive, fiz esse post através da minha própria aplicação. 
=)

> Observação: Para instalar como código executável, segue os seguintes comandos após clonar o repositório:
> `pip install pyinstaller`
> `pyinstaller -F tabnews.py`
> Use `-i` para inserir um ícone de sua escolha. `pyinstaller -i logo.ico -F tabnews.py`

## Imagens:
![](https://i.imgur.com/OPEdJ6O.png)
![](https://i.imgur.com/EtldFOc.png)
