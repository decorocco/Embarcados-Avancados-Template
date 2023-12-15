
# Tutorial SOC Linux Embarcado: Utilizando a Funcionalidade de Armazenamento em Massa USB no Raspberry Pi


- **Alunes:** Henrique Thomé, Rafael Libertini e André Rocco
- **Curso:** Engenharia da Computação
- **Semestre:** 10
- **Contato:** henriquert@al.insper.edu.br, rafaella2@al.insper.edu.br, andrebr@al.insper.edu.br
- **Ano:** 2023

## Começando

Bem-vindos, alunes de Engenharia da Computação! Neste emocionante tutorial, embarcaremos juntos em uma jornada para transformar o seu humilde Raspberry Pi em um poderoso dispositivo de armazenamento USB. Imagine a liberdade de desenvolver códigos para a FPGA diretamente no seu Raspberry Pi, sem depender de um PC dedicado. É hora de libertar o potencial de independência, flexibilidade e portabilidade que o Raspberry Pi oferece.

Para seguir esse tutorial é necessário:

- **Hardware:** Raspberry Pi (any model) 
- **Documentos:** [DE10-Standard_User_manual.pdf](https://github.com/Insper/DE10-Standard-v.1.3.0-SystemCD/tree/master/Manual)

## Motivação

Este tutorial tem como objetivo transformar o Raspberry Pi em um dispositivo de armazenamento USB, permitindo a construção de códigos para a FPGA no Raspberry Pi. As vantagens disso incluem:
   * Independência de Hardware: Não é necessário um PC dedicado para desenvolvimento; o Raspberry Pi serve como uma plataforma autônoma.
   * Flexibilidade: Permite desenvolver em qualquer sistema operacional que suporte conexão USB, incluindo macOS.
   * Portabilidade: O Raspberry Pi é pequeno e fácil de transportar, facilitando o trabalho em diferentes locais.


## 1. Armazenamento de Backup

O armazenamento de backup para o gadget de armazenamento precisa ser uma partição do disco e não pode ser um diretorio. 

- Um dispositivo inteiro, ex: `/dev/sda`.
- Uma única partição, ex: `/dev/sda1`.

### 2 Usando Uma Partição

Usar uma partição restringe o acesso do host USB apenas à partição especificada. Não há acesso a outras partes do dispositivo, incluindo a tabela de partição e o setor de boot do dispositivo.

#### 3 Criando um Armazenamento de Backup Baseado em Partição

As etapas são:

1. Certifique-se de estar trabalhando no dispositivo correto.
2. Reduza o tamanho da segunda partição para criar espaço livre necessário.
3. Crie uma nova partição primária no espaço livre. Deixe-a sem formatação.
4. Escreva as alterações no disco.

### 4 Particionando e Formatando o Armazenamento de Backup

Se você criou o armazenamento de backup com `mkfs`, esta etapa pode ser ignorada, a menos que você precise ou queira que ele contenha uma tabela de partição.

Para inicializar, particionar e formatar o armazenamento de backup no zero:

1. Anexe o armazenamento de backup a um dispositivo de loop:
   ```bash
   sudo losetup --show -fP /caminho/para/armazenamento-de-backup
   ```
2. Inicialize, particione e formate o armazenamento de backup usando as ferramentas normais do Linux (fdisk, gparted, etc) no dispositivo de loop criado no passo anterior.
3. Desanexe o armazenamento de backup do dispositivo de loop:
   ```bash
   sudo losetup -d /caminho/para/armazenamento-de-backup
   ```

## 5 Configurando o Zero

Esta configuração pode ser feita ao fazer login no zero ou montando seu cartão SD em um leitor conectado ao host USB.

### 5.1 Todos os Métodos

1. Faça backup do seu `config.txt`:
   ```bash
   sudo cp /boot/config.txt /boot/config.bak
   ```
2. Adicione o seguinte ao final de `/boot/config.txt`:
   ```
   [all]
   dtoverlay=dwc2,dr_mode=peripheral
   ```

### 5.2 Usando o Módulo g_mass_storage

#### 5.2.3 Como

1. Aplique os passos da seção 5.1.
2. Abra o crontab do root:
   ```bash
   sudo crontab -e
   ```
3. Adicione o seguinte no final do arquivo:
   ```
   @reboot /sbin/modprobe g_mass_storage file=/caminho/para/armazenamento-de-backup
   ```
4. Salve e feche.
5. Reinicie.

Este tutorial cobre os aspectos essenciais de usar a funcionalidade de armazenamento em massa USB em modelos de Raspberry Pi. Ideal para alunos interessados em SOC Linux Embarcado.
