
# Raspberry Pi Aos Poucos

-----

## Preparação e instalação do sistema

Este guia é sobre como instalar e configurar o sistema operacional Raspbian
Lite em um **Raspberry Pi 3 Model B**, conectado a internet via cabo de rede.
Usando outra máquina para gravar o sistema operacional no cartão SD e depois
terminar as configurações via SSH.

### 1. Requisitos:

- Cartão de memória micro SD
    -  velocidade: classe 10
    -  capacidade: pelo menos 8GB

- Computador 1
    - acesso a internet
    - capacidade de escrever em um cartão micro SD
    - terminal com SSH

- Computador 2
    - Raspberry Pi
    - Fonte de 5V, pelo menos 2A
    - conectado à rede via cabo Ethernet

Para esse tutorial não usarei monitor nem teclado no Pi, a ideia é ligá-lo na
rede e tomada apenas (o acesso a ele será via SSH por outra máquina). Então os
requisitos diferem um pouco do setup proposto na documentação oficial daqui:
https://www.raspberrypi.org/documentation/setup/

### 2. Download da imagem do sistema

Para esse tutorial escolhi o sistema **Raspbian Jessie Lite** pois o foco
inicial é montar um servidorzinho de tomada, como aplicativos com interface
gráfica e gerenciadores de Desktop não serão usados, uma distro leve e
minimalista serve, a imagem pode ser baixada em:
https://www.raspberrypi.org/downloads/raspbian/

![screen shot 2017-07-03 at 23 32
59](https://user-images.githubusercontent.com/7760/27812869-6d7d3e7c-6048-11e7-9847-0c91ee33a0d6.png)

### 3. Download e instalação do aplicativo de gravar imagens

Seguindo as recomendações da documentação oficial,
https://www.raspberrypi.org/documentation/installation/installing-images/README.md
usarei um aplicativo multiplataforma que facilita a gravação do cartão, o
Etcher https://etcher.io/

### 4. Gravar a imagem

Essa parte é simples, basta inserir o cartão no leitor, abrir o Etcher,
escolher a imagem (2017-06-21-raspbian-jessie-lite.zip), escolher o cartão e
clicar em **Flash**

![screen shot 2017-07-03 at 23 45 57](https://user-images.githubusercontent.com/7760/27813092-d0ccc370-6049-11e7-9c31-265700e5582b.png)

### 5. Criar um arquivo vazio de nome ssh na partição boot (FAT)

Uma vez gravado o SD terá 2 partições, uma "boot" com filesystem FAT32 e 
a outra "root" no formato ext4 que é a do sistema. Desde Novembro de 2016
o ssh não vem mais habilitado por padrão, e a maneira de ligar o ssh num
sistema "headless", sem tela, é simplesmente escrever um arquivo de nome 
``ssh``` sem extensão na raíz da partição "boot". Conforme documentado em
https://www.raspberrypi.org/documentation/remote-access/ssh/

No MacOS, ao inserir o sd no leitor apenas a partição boot será visível,
para criar esse arquivinho via Terminal você pode fazer assim:

```
cd /Volumes/boot
touch ssh
```

Feche a sessão do Terminal (Ctrl+D).

### 6. Colocar o SD no Pi e ligá-lo

Nesse ponto já deve estar tudo ok com o cartão, ejete-o do Computador 1 e insira-o
no Computador 2 (Pi), conecte o cabo de rede e ligue-o numa fonte mini USB
tipo de celular.

### 7. Primeiro acesso SSH

Aguarde um tempo para o Pi bootar, e volte para o Computador 1, para conectar
no seu Pi você pode ou achar o IP local dele procurando as maquinas conectadas
na pagina de admin do seu roteador, ou tentar usar o hostname padrao "raspberrypi"
seguido de ".local"

O usuário padrão é pi, com senha raspberry

```
ssh pi@raspberrypi.local
```

Como é a primeira vez que você conecta ele vai te mostrar o fingerprint da
maquina e perguntar se você quer adicionar nos seus know_hosts, digite yes:

```
The authenticity of host 'raspberrypi.local (...)' can't be established.
ECDSA key fingerprint is SHA256:....
Are you sure you want to continue connecting (yes/no)? yes
```

Ao fazer o login, digite passwd para mudar a senha padrão, digite a padrão e
depois a nova duas vezes.

```
pi@raspberrypi:~ $ passwd
Changing password for pi.
(current) UNIX password:
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
```

### 8. Habilitar SSH automaticamente nos próximos boots

Depois que o Pi liga e habilita o ssh, ele apaga o arquivo ssh que você copiou
para a partição boot, isso significa que se você deesligar a maquina, o ssh nao
irá levantar sozinho de novo, você terá que tirar o cartao, criar o arquivinho
de novo e colocar de novo. (ou plugar um keyboard e tv e ligar usando sudo raspi-config)

Para evitar isso, uma vez que você logar via ssh a primeira vez, você pode habilitar
ele permanente usando o systemd dessa forma:

```
sudo systemctl enable ssh
```

Lembrando que deixar o ssh sempre ligado pode ser uma porta de entrada para gatunos,
principalmente se voce nao mudou a senha padrão. 

Além de mudar a senha padrão, o ideal mesmo é desabilitar o login via ssh com 
senha. Na sessão "Próximos Passos" tem um link de como configurar o login sem
password/autenticação baseada em chaves.

### 9. Atualizando o sistema

https://www.raspberrypi.org/documentation/raspbian/updating.md

```
sudo apt-get update
sudo apt-get dist-upgrade
sudo apt-get clean
```

### 10. Próximos passos

Algumas sugestões do que fazer em seguida:

- Mudar a Timezone e locale
    - sudo raspi-config
- Remover o password do ssh e permitir só uma lista de máquinas/chaves conhecidas
    - https://www.raspberrypi.org/documentation/configuration/security.md e
    - https://www.raspberrypi.org/documentation/remote-access/ssh/passwordless.md
    - depois:
        - sudo nano /etc/ssh/sshd_config
            - PasswordAuthentication no
        - sudo /etc/init.d/ssh restart
- Instalar ferramentas uteis
    - vim
    - git
- Aumentar swap (para compilar programas maiores)
    - https://www.bitpi.co/2015/02/11/how-to-change-raspberry-pis-swapfile-size-on-rasbian/
- Configurar TOR?
- Configurar VPN?
- Instalar e rodar um full node de bitcoin
- Configurar Wifi?
    - https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md








