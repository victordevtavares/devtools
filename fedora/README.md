
# Fedora

## Criar Alias:
alias c="clear" -> clean monitor

## Wifi:
nmcli dev wifi -> show available wifis

nmcli dev wifi connect WIFI_NAME password WIFI_PASSWORD

## Change locale of keyboard:

localectl --no-convert set-x11-keymap XXX 
    -> where XXX can be: 
        us
        br 

## Reset locked user centos
pam_tally2 --user=USERNAME --reset

## Control sshd.service:
systemctl XXX sshd.service
    -> where XXX can be:
        start
        stop
        restart
        enable
        disable

## Test if some port is available on remote:
netstat -ltnup | grep 'IP:PORT'

## Comandos aleatorios:
history: mostra o histórico de comandos executados pelo usuário atual;
sudo: concede permissão de SU para execução de determinado comando;
mysqldump: cria uma cópia do banco de dados mysql (requer parâmetros adicionais - pesquisar);
scp -p USUARIO@SERVER:/DIRETÓRIO_ORIGEM/ /DIRETÓRIO_DESTINO/ : copia arquivos de um servidor para a máquina local -> -r para recursivo, -p para manter aspecto padrão;
scp -p /DIRETÓRIO_ORIGEM/ USUARIO@SERVER:/DIRETÓRIO_DESTINO/: copia arquivos da máquina local para um servidor -> -r para recursivo, -p para manter aspecto padrão;
curl DOMÍNIO: executa o navegador;
nano ARQUIVO: editor de texto;
sl -la: exibe as permissões detalhadas dos arquivos;
chmod: altera as permissões de leitura escrita e execução;
chown: altera o dono do diretório/arquivo;
chgrp: altera o grupo do diretório/arquivo;
reboot -h now: reinicia o servidor encerrando os processos antes (evita problemas de inicialização);
pwd: mostra o diretório atual
iptables -L --line-numbers: lista as permissões de firewall ativas;
nano /etc/sysconfig/iptables: edita as configurações padrão de iptables (carrega todo boot ou quando usado o iptables-restore);
iptables-restore < /etc/sysconfig/iptables
https://www.digitalocean.com/community/tutorials/iptables-essentials-common-firewall-rules-and-commands - > mais comandos de iptables
CUIDADO: iptables -D INPUT 5: apaga a linha 5 do line-numbers;
ps -eaf: lista todos os serviços em execução;
df -h: exibe os discos montados;
du -sh <directory>: disk usage - mostra o tamanho do diretório;
du -h -d1 <directory> : mostra o tamanho das subpastas do primeiro nível do diretório selecionado 
fdisk -l: mostra as partições montadas com info do S. O.
awk -F':' '{ print $1}' /etc/passwd: mostra todos os usuários registrados no S. O.
passwd <username>: muda a senha de um <username>
runuser -l  userNameHere -c 'command’: run commands as another user
tail -f <ARQUIVO>: Visualização de arquivo em tempo real
adb devices: mostra os dispositivos conectados ao computador;
find . -name testfile.txt - Busca arquivo
grep -rnw . -e "texto" - find all files containing specific text on Linux
hostnamectl: exibe as informações do sistema operacional;
sestatus: exibe o status do selinux (firewall do CentOS Distr Red)
crontab -l: exibe os agendamentos registrados no sistema;
netstat -tulpn: checar quem está usando as portas do sistema
 _________________________________________________________________________
## Checagem de Memória e Processos

“top”: check memory and cpu usage per process
“htop”: shows memory usage along with various other details
“free - m”: show a short resume of used/free memory
“vmstat -s”: lays out the memory usage statistics 
 _________________________________________________________________________
## RSYNC
Sintaxe básica:
rsync <opções> <origem> <destino> 

Instalar:
apt-get install rsync

ALGUMAS OPÇÕES DO COMANDOS RSYNC:
-V: verbose
-R: cópias de dados de forma recursiva (mas não preservam timestamps e permissão durante a transferência de dados
-A: modo de arquivamento, o modo de arquivo permite a cópia de arquivos de forma recursiva e também preserva links simbólicos, permissões de arquivos, posses usuário e grupo e timestamps
-Z: arquivos serão comprimidos
-H: legíveis, saída em um formato legível para humano (esse é muito bom)

EXEMPLOS:

rsync -zvh planilha.xls /tmp/backups/
Este comando irá sincronizar um único arquivo em uma máquina local. Aqui neste exemplo, o arquivo planilha.xls está sendo copiado / sincronizado para o diretório /tmp/backups 

rsync -avzh  /var/apt/apt/archives /tmp/backups/
O comando acima irá transferir ou sincronizar todos os arquivos de um diretório para outro diretório na mesma máquina. Aqui neste exemplo, /var/apt/apt/archives contém alguns arquivos de pacotes .deb

_________________________________________________________________________
Equivalencia de Permissões
￼
0 is no read/write/execute or search access
1 is execute/search
2 is write
3 is write/execute or search
4 is read
5 is read/execute or search
6 is read/write
7 is read/write/execute or search
					
To change all the directories to certain permission (recommended 755 for public directories):
find /DIRECTORY/ -type d -exec chmod 755 {} \;
find . -type d -exec chmod 755 {} \;

To change all the files to certain permission (recommended 644 for public files):
find /FILE/ -type f -exec chmod 644 {} \;
find . -type f -exec chmod 644 {} \;

_________________________________________________________________________
## Configurar SFTP Limitado por pasta

https://www.thegeekdiary.com/how-to-configure-separate-port-for-ssh-and-sftp-on-centos-rhel/
Bizú: Criar uma pasta Home e fazer um mount --bind final, já que o CHRoot vai permitir a visualização do diretório acima da pasta do usuário
_________________________________________________________________________
## Destravar autolock LibreOffice

File Locking
File locking is enabled by default in LibreOffice. On a network that uses the Network File System protocol (NFS), the locking daemon for NFS clients must be active. To disable file locking, edit the soffice script and change the line "export SAL_ENABLE_FILE_LOCKING" to "# export SAL_ENABLE_FILE_LOCKING". If you disable file locking, the write access of a document is not restricted to the user who first opens the document.
Warning: The activated file locking feature can cause problems with Solaris 2.5.1 and 2.7 used in conjunction with Linux NFS 2.0. If your system environment has these parameters, we strongly recommend that you avoid using the file locking feature. Otherwise, LibreOffice will hang when you try to open a file from a NFS mounted directory from a Linux computer.
_________________________________________________________________________
## Bash exibir histórico:
echo 'export HISTTIMEFORMAT="%d/%m/%y %T "' >> ~/.bashrc
source ~/.bashrc
_________________________________________________________________________
## Regras Padrão Effetive para iptables:
/etc/sysconfig/iptables

iptables -A INPUT -p tcp --tcp-flags ALL NONE -j DROP
iptables -A INPUT -p tcp ! --syn -m state --state NEW -j DROP
iptables -A INPUT -p tcp --tcp-flags ALL ALL -j DROP
iptables -A INPUT -i lo -j ACCEPT
iptables -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT
iptables -A INPUT -p tcp -m tcp --dport 22 -j ACCEPT
iptables -A INPUT -p tcp -m tcp --dport 8080 -j ACCEPT

ou:

firewall-cmd --zone=public --add-port=9001/tcp --permanent
firewall-cmd --reload

Listar os usuários do Linux
cut -d: -f1 /etc/passwd

Adicionar usuário a grupo linux
gpasswd -a USER GROUP

Remover usuário a grupo linux
gpasswd -d USER GROUP

_____________________________________
## Installing Apache2 , Mysql server and PHP on Centos
https://www.krizna.com/centos/installing-apache2-mysql-server-php-centos-6-lamp/
_____________________________________
## How To Install Jenkins on CentOS 7
https://linuxize.com/post/how-to-install-jenkins-on-centos-7/
_____________________________________
## How To Install An Rpm File On Linux Os (Centos, Rhel, & Fedora)
https://phoenixnap.com/kb/how-to-install-rpm-file-centos-linux
_____________________________________
## Compress Folder
tar -zcvf archive-name.tar.gz directory-name
_____________________________________
## Formatar e montar partições no Linux

fdisk -l
lista as partições montadas

fdisk /etc/sdX
para criar partições no disco -> n -> p -> 1 (ou enter) -> w (para salvar)

mkfs.ext4 /etc/sdX1
para formatar com ext4

nano /etc/fstab  -> /dev/sdX1	/opt/tomcat/webapps	ext4	 	defaults		0 0 (adicionar na última linha)
para automount

mount -a
ele vai testar antes de executar o fstab. Caso dê erro, não buga o sistema !!! importantíssimo !!!
_____________________________________
## Instalar Micro Centos 7

yum install epel-release
yum install snapd -y
systemctl enable --now snapd.socket
ln -s /var/lib/snapd/snap /snap
*caso seja necessário proxy: 
	nano /etc/sysconfig/snapd  -> adicionar o proxy http e https
snap wait system seed.loaded
snap install micro --classic
yum install xclip xsel -y
caso o micro não responda quando chamado:
	echo "export PATH=$PATH:/snap/bin" >> ~/.bashrc
	source ~/.bashrc