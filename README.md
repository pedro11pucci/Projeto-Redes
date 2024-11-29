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
### Diagrama de topologia da rede:
![389943997-0115937f-9e65-4ad2-b1eb-cd89a6e64876](https://github.com/user-attachments/assets/a1f198c7-177c-442c-9908-174094710e78)

