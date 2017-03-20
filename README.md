# Cadprovedor dominio provedor reverso
#Bye Samuelfernandes
#! /bin/sh
diretorio=/var/spool/alpha/diretorios/
DIRdisco=/var/spool/alpha/diretorios/
cadastrar(){


if [ -f "$diretorio/alpha/cadastro.txt" ]; then
  DOMINIO=$(cat $diretorio/alpha/cadastro.txt 2> outerr | grep 'dominio:' | sed 's/dominio://')
  NOME=$(cat $diretorio/alpha/cadastro.txt 2> outerr | grep 'nome:' | sed 's/nome://')
  
else
  DOMINIO='meuprovedor.com'
  NOME='MEUPROVEDOR'
fi

while :
do
	DOMINIOPROV=""
	NOMEPROV=""
	cancel=0
	while [ ! "$DOMINIOPROV" ]; do
		DOMINIOPROV=$(dialog --stdout --title "CADASTRO PROVEDOR" --inputbox "DOMINIO:" 0 0 "$DOMINIO")
		[ $? -eq 1 ] && { cancel=1; break; }
		#NOMEPROV=$(echo $NOMEPROV | grep '@.*[.]')
		DOMINIOPROVINVA=$(echo $DOMINIOPROV | egrep '(\ |\/|"|\\|\%|\$|\!|\*|\/|\,|\;|\?|\[|\]|\{|\}|\:|\^|\~|\(|\)|\#)')
		 if [ "$DOMINIOPROV" ] && [ ! "$DOMINIOPROVINVA" ]; then
		    break
		 else   
		    dialog --msgbox "\nInvalido, campo em branco ou caractere especiais\n\n" 0 0
		    DOMINIOPROV=""
		    DOMINIO=""
		 fi
	done
	[ $cancel -eq 1 ] && break

	cancel=0
    while [ ! "$NOMEPROV" ]; do
	NOMEPROV=$(dialog --stdout --title "CADASTRO PROVEDOR" --inputbox "NOME:" 0 0 "$NOME")
	[ $? -eq 1 ] && { cancel=1; break; }
	#NOMEPROV=$(echo $NOMEPROV | grep '@.*[.]')
	NOMEPROVINVA=$(echo $NOMEPROV | egrep '(\/|\\|\%|\$|\!|\*|\"|\,|\;|\?|\[|\]|\{|\}|\:|\^|\~|\(|\)|\#)')
	if [ "$NOMEPROV" ] && [ ! "$NOMEPROVINVA" ]; then
	  #altera espaco em branco para +
	  
	   echo 'dominio:'$DOMINIOPROV > $diretorio/thome/cadastro.txt
	   echo 'nome:'$NOMEPROV >> $diretorio/thome/cadastro.txt
	   
cp $diretorio/alpha/cadastro.txt /var/www/mini/meuip.com/ 2> outerr
cp $diretorio/alpha/cadastro.txt /var/www/mini/meuip.com.br/ 2> outerr
cp $diretorio/alpha/cadastro.txt /var/www/mini/meuenderecoip.com/ 2> outerr
cp $diretorio/alpha/cadastro.txt /var/www/mini/miip.mx/ 2> outerr
cp $diretorio/alpha/cadastro.txt /var/www/mini/algartelecom.com.br/ 2> outerr
cp $diretorio/alpha/cadastro.txt /var/www/mini/localizaip.com.br/ 2> outerr

#sed -i 's/'$DOMINIO'/'$DOMINIOPROV'/g' /var/www/mini/meuip.com/index.php 2> outerr
#sed -i 's/'$NOME'/'$NOMEPROV'/g' /var/www/mini/meuip.com/index.php 2> outerr

#sed -i 's/'$DOMINIO'/'$DOMINIOPROV'/g' /var/www/mini/meuip.com.br/index.php 2> outerr
#sed -i 's/'$NOME'/'$NOMEPROV'/g' /var/www/mini/meuip.com.br/index.php 2> outerr

#sed -i 's/'$DOMINIO'/'$DOMINIOPROV'/g' /var/www/mini/meuenderecoip.com/index.php 2> outerr
#sed -i 's/'$NOME'/'$NOMEPROV'/g' /var/www/mini/meuenderecoip.com/index.php 2> outerr

#sed -i 's/'$DOMINIO'/'$DOMINIOPROV'/g' /var/www/mini/miip.mx/index.php 2> outerr
#sed -i 's/'$NOME'/'$NOMEPROV'/g' /var/www/mini/miip.mx/index.php 2> outerr

#sed -i 's/'$DOMINIO'/'$DOMINIOPROV'/g' /var/www/mini/algartelecom.com.br/index.php 2> outerr
#sed -i 's/'$NOME'/'$NOMEPROV'/g' /var/www/mini/algartelecom.com.br/index.php 2> outerr

#sed -i 's/'$DOMINIO'/'$DOMINIOPROV'/g' /var/www/mini/localizaip.com.br/index.php 2> outerr
#sed -i 's/'$NOME'/'$NOMEPROV'/g' /var/www/mini/localizaip.com.br/index.php 2> outerr


echo '
<?
$url = file_get_contents('\''http://www.localizaip.com.br/api/iplocation.php'\'');
$content = preg_replace('\''/provider":"(.*)","latitude/'\'','\''provider":"'$NOMEPROV'","latitude'\'',$url);

$ip = preg_replace('\''/(.*)ip\":\"/'\'','\'''\'',$content);
$ip = preg_replace('\''/\",(.*)/'\'','\'''\'',$ip);

$content = preg_replace('\''/reverseDNS":"(.*)","src/'\'','\''reverseDNS":"'\''.$ip.'\''.'$DOMINIOPROV'","src'\'',$content);

echo $content;
?>
' 2> out > /var/www/mini/iplocation.php 

dialog  --title "Redirecionar" --yesno "\nRedirecionar simet.nic.br e nperf.com para speedtest.net e whatismyipaddress.com para meuip.com ?\n\n" 0 0
if [ $? = 0 ]; then
 touch /usr/local/man/.redirect 2> outerr
else
 rm /usr/local/man/.redirect 2> outerr
fi

dialog  --title "speedtest.net" --yesno "\nSpeedtest.net em Portugues ?\n\n" 0 0
dialog  --infobox "\nAguarde processando ..\n\n" 0 0
if [ $? = 0 ]; then
   wget -T 10 -t 2 "http://www.speedtest.net/pt/speedtest-config.php" > out 2> outerr
   wget -t 10 -t 2 "http://api.ookla.com/ipaddress.php" > out 2> out
   
   NOME_copel=$(echo $NOMEPROV | tr ' ' '+')
   [ -f "ipaddress.php" ] && { sed -i 's/isp=.*/isp='$NOME_copel'/g' ipaddress.php; echo '<? echo "'$(cat ipaddress.php)'"; ?>' 2> out > /var/www/mini/ipaddress.php; } 
   
   if [ -f "speedtest-config.php" ]; then 
      NOME_speed=$(echo $NOMEPROV | tr ' ' '_')
      mv speedtest-config.php /var/www/mini/speedtest-config.xml 2> outerr

      sed -i 's/isp=\".*\" isprating/isp=\"'$NOME_speed'\" isprating/g' /var/www/mini/speedtest-config.xml 2> outerr
  
      sed -i 's/isp_name=\".*\" isp_common_name=\".*\"\//isp_name=\"'$NOME_speed'\" isp_common_name=\"'$NOME_speed'\"\//g' /var/www/mini/speedtest-config.xml 2> outerr

   fi
   
   touch $DIRdisco/thome/speedtestPT 2> outerr
   LINGU="pt-br"
else
   wget -T 10 -t 2 "http://www.speedtest.net/speedtest-config.php" > out 2> outerr
   wget -t 10 -t 2 "http://api.ookla.com/ipaddress.php" > out 2> out
   
   NOME_copel=$(echo $NOMEPROV | tr ' ' '+')
   [ -f "ipaddress.php" ] && { sed -i 's/isp=.*/isp='$NOME_copel'/g' ipaddress.php; echo '<? echo "'$(cat ipaddress.php)'"; ?>' 2> out > /var/www/mini/ipaddress.php; } 
   
   if [ -f "speedtest-config.php" ]; then 
      NOME_speed=$(echo $NOMEPROV | tr ' ' '_')
      mv speedtest-config.php /var/www/mini/speedtest-config.xml 2> outerr

      sed -i 's/isp=\".*\" isprating/isp=\"'$NOME_speed'\" isprating/g' /var/www/mini/speedtest-config.xml 2> outerr
  
      sed -i 's/isp_name=\".*\" isp_common_name=\".*\"\//isp_name=\"'$NOME_speed'\" isp_common_name=\"'$NOME_speed'\"\//g' /var/www/mini/speedtest-config.xml 2> outerr

   fi
   
   rm $DIRdisco/thome/speedtestPT 2> outerr
   LINGU="en"
fi


if [ -f "/var/www/mini/speedtest-config.xml" ]; then
  sed -i 's/isp=\".*\" isprating/isp=\"'$NOMEPROV'\" isprating/g' /var/www/mini/speedtest-config.xml 2> outerr

  sed -i 's/isp_name=\".*\" isp_common_name=\".*\"\//isp_name=\"'$NOMEPROV'\" isp_common_name=\"'$NOMEPROV'\"\//g' /var/www/mini/speedtest-config.xml 2> outerr
fi

    rm ipaddress.php 2> out
    rm speedtest-config.php 2> out

touch /usr/local/man/.atva 2> outerr
$DIRdisco/thome/funcoes/mod_StRd "redirect" > outerr 2> outerr 

#sed -i 's/\"provider=.*\$/\"provider='$NOMEPROV'\$/g' $DIRdisco/thome/funcoes/redirect.pl 2> outerr

killall -9 unbound 2> outerr
killall -9 squid 2> outerr
RespostaSquid


           dialog --msgbox "\n\nDOMINIO: $DOMINIOPROV\n\nNOME: $NOMEPROV\n\n" 0 0
		    
		    cancel=1; break
	else   
	   dialog --msgbox "\nInvalido, campo em branco ou caractere especiais\n\n" 0 0
	   NOMEPROV=""
	   NOME=""
	fi
     done
	[ $cancel -eq 1 ] && break
done
}

RespostaSquid(){
if [ -f "/usr/local/squid/sbin/squid" ] && [ -f "/usr/sbin/squid" ]; then

while :
do

PROXYL=$(ps aux | egrep '\(squid\)' | sed 's/proxy *//; s/ .*//')
PROXYS=$(pgrep -f squid-1)

if [ "$PROXYL" ] && [ "$PROXYS" ]; then
 break
fi
sleep 1
done

fi

}


principal(){
[ "$1" = "cadastrar" ] && { cadastrar; }
}

principal ${1%}
