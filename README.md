# Trabalho Prático: Criação de Infraestrutura de Rede para a Empresa XPTO

## Objetivo:
Desenvolver uma infraestrutura de rede para a empresa fictícia XPTO, que atenda aos seguinte requisitos:
<ol>
  <li><b>Load Balancer</b>:  Implementar um sistema de balanceamento de carga utilizando, no mínimo, 3 máquinas para distribuir o tráfego. </li>
  <li><b>Proxy Reverso</b>: Configurar uma máquina para atuar como proxy reverso, gerenciando as requisições dos usuários para os servidores apropriados.</li>
  <li><b>Banco de Dados</b>: Implementar uma máquina dedicada para o banco de dados, garantindo a segurança e integridade dos dados. </li>
  <li><b>VPN</b>: Configurar o acesso à rede através de uma VPN, garantindo que todas as comunicações sejam seguras.</li>
  <li><b>Docker</b>: Utilizar o Docker para criar os servidores web e o banco de dados, garantindo portabilidade e fácil gestão dos serviços.</li>
</ol>

## Requisitos do Trabalho

1. **Arquitetura da Rede:**
   - Desenhar a topologia da rede, identificando as máquinas, conexões e fluxos de dados.
   - Detalhar o uso do load balancer, proxy reverso e banco de dados.

2. **Configuração do Load Balancer:**
   - Implementar um load balancer utilizando Nginx.
   - Configurar para balancear o tráfego entre, no mínimo, 3 máquinas que servirão conteúdo web.

3. **Proxy Reverso:**
   - Configurar uma máquina com Nginx para atuar como proxy reverso.
   - Detalhar como o proxy reverso irá gerenciar as requisições e redirecioná-las para as máquinas corretas.

4. **Banco de Dados:**
   - Criar uma máquina para o banco de dados utilizando o Docker ou AWS RDS.
   - Implementar e configurar um banco de dados relacional ou não relacional (MySQL, PostgreSQL, MongoDB etc.) dentro de um container Docker ou AWS RDS.
   - Detalhar as medidas de segurança adotadas para proteger os dados armazenados.

5. **VPN:**
   - Configurar uma VPN (como OpenVPN) para que o acesso à rede da empresa XPTO seja seguro.
   - Demonstrar o processo de configuração da VPN e como ela se integra com o restante da infraestrutura.

6. **Docker:**
   - Utilizar o Docker para criar e gerenciar os containers dos servidores web e do banco de dados.
   - Explicar a configuração dos containers e como eles se comunicam entre si.
   - Demonstrar como os containers podem ser escalados ou migrados para outros ambientes.

## Entrega: 

### Relatório Técnico:
#### Diagrama de topologia da rede:
![389943997-0115937f-9e65-4ad2-b1eb-cd89a6e64876](https://github.com/user-attachments/assets/a1f198c7-177c-442c-9908-174094710e78)

### Criação e Configuração de Instâncias EC2 para o Load Balancer

#### Criação da Instância EC2
1. Crie uma instância EC2 com uma chave de acesso.
2. Configure o **Grupo de Segurança** da instância para que as **Regras de Entrada** permitam acesso geral.

---

#### Acessar a instância com SSH
Após criar a instância, acesse-a usando o comando:

```bash
ssh -i <caminho/da/chave> <usuario>@<ip-da-maquina>
```

#### Atualizar e Instalar o Nginx
1. Após acessar a máquina, atualize o repositório apt e instale o Nginx com os seguintes comandos:
```bash
sudo apt update
sudo apt install nginx
```

#### Configurar o Nginx
1. Crie e edite o arquivo de configuração do Nginx:
```bash
sudo vim /etc/nginx/conf.d/redes.conf
```

2. Adicione o seguinte conteúdo ao arquivo:
```bash
upstream frontend_servers {
    server <IP_do_servidor_web_1>;
    server <IP_do_servidor_web_2>;
    server <IP_do_servidor_web_3>;
}

server {
    listen 80;
    server_name <IP_da_maquina_Nginx>;

    location / {
        proxy_pass http://frontend_servers;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

#### Reiniciar o Serviço do Nginx
1. Reinicie o serviço do Nginx para aplicar as mundaças:
```bash
sudo systemctl restart nginx
```

### Criação e Configuração dos Servidores Web

#### Criar o Arquivo HTML
1. Crie um arquivo HTML chamado `index.html` com o seguinte conteúdo:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mensagens</title>
    <script>
        const apiUrl = 'http://<ip-do-servidor-backend>:8080/api/mensagem';

        async function fetchMessages() {
            try {
                const response = await fetch(apiUrl, {
                    method: 'GET',
                    headers: { 'Content-Type': 'application/json' },
                    credentials: 'same-origin'
                });
                if (!response.ok) {
                    throw new Error('Erro ao buscar mensagens');
                }
                const messages = await response.json();
                displayMessages(messages);
            } catch (error) {
                console.error(error);
            }
        }

        function displayMessages(messages) {
            const messageList = document.getElementById('messageList');
            messageList.innerHTML = '';
            messages.forEach(message => {
                const listItem = document.createElement('li');
                listItem.textContent = `${message.data}: ${message.mensagem}`;
                messageList.appendChild(listItem);
            });
        }

        async function sendMessage() {
            const messageInput = document.getElementById('messageInput');
            const mensagem = messageInput.value;
            const data = new Date().toISOString().split('T')[0];

            try {
                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({ mensagem, data })
                });

                if (!response.ok) {
                    throw new Error('Erro ao enviar mensagem');
                }

                messageInput.value = '';
                fetchMessages();
            } catch (error) {
                console.error(error);
            }
        }

        window.onload = fetchMessages;
    </script>
</head>
<body>
    <main>
        <div>
            <h1>Aplicação</h1>
            <ul id="messageList"></ul>
            <input type="text" id="messageInput" placeholder="Digite sua mensagem" />
            <button onclick="sendMessage()">Enviar</button>
        </div>
    </main>
</body>
</html>
```

#### Criação das Instâncias EC2
1. Crie <strong>3 instâncias EC2</strong> com a configuração necessária.
2. Acesse uma das instâncias
```bash
ssh -i <caminho/da/chave> <usuario>@<ip-da-maquina>
```

#### Instalar o Nginx
1. Após acessar uma das instâncias, atualize o repositório <strong>apt</strong> e instale o Nginx:
```bash
sudo apt update
sudo apt-get install nginx
```

#### Copiar o arquivo HTML para o diretório do Nginx
1. Copie o arquivo HTML da sua máquina local para o diretório do Nginx na instância EC2:
```bash
scp -i <caminho/da/chave> <caminho/do/html> <usuario>@<ip-da-maquina>:/var/www/html
```

#### Reiniciar o Serviço do Nginx
1. Após copiar o arquivo, reinicie o serviço do Nginx para que ele sirva o arquivo:
```bash
sudo systemctl restart nginx
```

#### Repetir o processo
1. Repita os passos de Atualização do Nginx, Cópia do HTML, e Reinício do Serviço para as outras duas máquinas.
