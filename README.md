# CONFIGURAR-LIRC-NO-RASPBERRY-KERNEL-4.19
CONFIGURAR LIRC 0.9.4c-9 NO RASPBERRY STRETCH KERNEL 4.19

1° INSTALAR O LIRC:
Instalação manual do lirc 0.9.4c-9 no raspbian stretch:
```
sudo dpkg –i <PACOTE>
```
Siga a seguinte ordem de instalação dos pacotes:
- [1]    libusb-0.1-4_0.1.12-30_armhf.deb
- [2]    libftdi1-2_1.3-2+b2_armhf.deb
- [3]    liblirc0_0.9.4c-9_armhf.deb
- [4]    libportaudio2_19.6.0-1_armhf.deb
- [5]    lirc_0.9.4c-9_armhf.deb

Instalação direta pelo terminal:
```
sudo apt-get install lirc
```
2° Agora edite o arquivo /boot/config.txt , onde vamos fazer os módulos do lirc iniciar no boot, configurando os devidos pinos GPIO usados.
```
sudo nano /boot/config.txt
```
Procure pela linha “#dtoverlay=lirc-rpi”, você pode excluí-la e deixar da seguinte forma, ou deixa-la comentada e adicionar as seguintes linhas no arquivo:
```
dtoverlay=gpio-ir,gpio_pin=6
dtoverlay=gpio-ir-tx,gpio_pin=12
```
gpio-ir = Para o pino de recebimento de dados(RECEPTOR IR)

gpio-ir-tx = Para o pino de saída de dados(LED IR)
*Salve e feche o arquivo.

3° Agora vamos configurar os arquivos do lirc, vamos editar o arquivo /etc/lirc/lirc_options.conf, procure pelas linhas “Driver = ...” e “Device =  ....” e deixe da seguinte forma:
```
Driver = default
Device = /dev/lirc0
```
*Salve e feche o arquivo.

### Não edite ou adicione o arquivo hardware.conf, outros tutorias podem mencionar este aquivo em /etc/lirc/hardware.conf, o lirc na versão 0.9.4 não utiliza mais este arquivo.

4° TESTAR
Vamos primeiro reiniciar o serviço do lirc para evitar erros:
```
sudo service lircd restart
```
Agora vamos reiniciar o respberry:
```
sudo reboot
```

*Após reiniciado, vamos procurar pelas mensagens do boot relacionadas ao lirc:
```
dmesg | grep lirc
```
*Agora vamos listar os device do lirc
```
ls –l /dev/lirc*
```

*se todos os comandos tiveram saídas positivas, esta tudo certo.
*Para testar se o receptor esta ok, use o comando mode2 para que ele leia pulsos ir, então basta executar o comando a seguir e apertar botões de algum controle IR mirando no receiver IR.
```
mode2
```
Ou
```
mode2 –d /dev/lirc0
```
