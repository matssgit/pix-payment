

***

# Pix Payment

Pix Payment é uma aplicação web simples e didática para geração, visualização e controle de pagamentos via QR Code Pix, construída com **Flask**, **Flask-SocketIO**, **SQLAlchemy**, **pytest** e front-end responsivo com templates Jinja2.

## ⚡️ Funcionalidades

- Criação de cobrança Pix (simulado), com geração de QR Code estático único.
- Registro do pagamento com valor, status, data de expiração e manhã identificação bancária.
- Visualização web do QR Code para pagamento.
- Confirmação de pagamento via API (simulando retorno do banco, com websocket para UI em tempo real).
- Página de confirmação dinâmica após o pagamento.
- Página customizada 404 para cobranças não encontradas.
- Testes automatizados do módulo Pix e geração de QR Codes.
- Estrutura organizada para expansão futura (integração real com bancos, autenticação, etc).

***

## 🏗️ Estrutura de Pastas

```
PixPayment/
│
├── app.py                  # Aplicação Flask principal, registro das rotas e lógica central
├── repository/
│   └── database.py         # Instância e configuração do SQLAlchemy (db)
├── db_models/
│   └── payment.py          # Modelo (ORM) Payment
├── payments/
│   └── pix.py              # Classe Pix: gera identificador e arquivo QR Code
├── tests/
│   └── test_pix.py         # Testes automatizados de criação Pix
├── static/
│   ├── css/styles.css      # Estilos customizados
│   └── template_img/       # SVG/PNG de ícones
│       ├── tag.svg
│       ├── check.svg
│       ├── clock.svg
│       ├── basket.svg
│       ├── 404.png
│       └── qrcode.png
│   └── img/                # QR Codes gerados dinamicamente (por cobrança)
├── templates/
│   ├── payment.html        # Página principal de pagamento Pix (QR e status aguardando)
│   ├── confirmed_payment.html # Página de confirmação após pagamento (QR + ícone check)
│   └── 404.html            # Página de erro/cobrança não encontrada
└── instance/database.db    # Banco de dados SQLite

```

***

## 🚀 Como rodar localmente

### Pré-requisitos

- Python 3.9+
- Virtualenv (opcional, mas recomendado)

### Passos

1. **Clone o repositório**
   ```bash
   git clone https://github.com/seuusuario/pix-payment.git
   cd pix-payment
   ```

2. **(Opcional) Ative o virtualenv**
   ```bash
   python -m venv .venv
   source .venv/bin/activate # ou .venv\Scripts\activate no Windows
   ```

3. **Instale dependências**
   ```bash
   pip install -r requirements.txt
   ```

   Exemplo de requirements.txt:
   ```
   Flask
   Flask-SocketIO
   flask_sqlalchemy
   qrcode[pil]
   pytest
   ```

4. **Rode o servidor**
   ```bash
   python app.py
   # ou
   flask run
   ```

5. **Acesse o app**
   ```
   http://127.0.0.1:5000/payments/pix/1
   ```
   (substitua `1` pelo ID real criado)

***

## 🧩 Fluxo completo

1. **Criar cobrança Pix**  
   Envie um POST:
   ```
   POST /payments/pix
   Body: { "value": 123.45 }
   ```
   Resposta: JSON com `bank_payment_id`, `qr_code_path`, e outros dados.

2. **Visualizar o QR code**  
   Use o ID retornado:
   ```
   GET /payments/pix/<payment_id>
   ```
   Abre página com QR code, valor e status.

3. **Confirmar pagamento (simulado)**  
   Envie um POST de confirmação:
   ```
   POST /payments/pix/confirmation
   Body: {
       "bank_payment_id": "<copie do JSON de criação>",
       "value": 123.45
   }
   ```
   No front-end a confirmação aparece em tempo real via websocket.

***

## 🖼️ Detalhes técnicos

- **Rota `/payments/pix/qr_code/<file_name>`** serve a imagem PNG do QR code gerado.
- **O nome salvo no banco deve bater com o nome do arquivo gerado.**
- **Front-end**: Todos os ícones e SVG são servidos via `url_for('static', ...)`.
- **Websockets**: Não é requerida configuração extra para desenvolvimento local, só rodar.
- **Página 404**: Mostrada quando um `payment_id` inválido é acessado via browser.

***

## 🧪 Executando testes

Para testar a geração de pagamento Pix/QR Code:

```bash
pytest tests/test_pix.py
```

***

## 🦾 Dicas para produção

- Use um servidor WSGI (ex: gunicorn) e servidor de websocket adequado para produção.
- Proteja as rotas sensíveis se usar dados reais!
- Adapte o model para logs extras de auditoria ou integrações futuras.

***

## 👨‍💻 Autor e Licença

Desenvolvido por Matheus Santana 

Licença: MIT

***
