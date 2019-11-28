```PowerShell
function mostrarMenu 
{ 
     param ( 
           [string]$Titulo = 'BOLSA' 
     ) 
     cls 
     Write-Host "================ $Titulo ================" 
      
     
     Write-Host "1.Bolsa en subida"
     Write-Host "2.Bolsa en bajada"
     Write-Host "3.Bolsa estancada" 
     Write-Host "4.Salir" 
}

do 
{ 
     mostrarMenu
     $input = Read-Host "¿Qué dominios desea crear?"
     switch ($input) 
     { 
'1' { 
#Sacar contenido de la web y meterlo en un archivo
cd D:\xampp\htdocs
$web = iwr "http://www.bolsamadrid.es/esp/aspx/Mercados/Precios.aspx?indice=ESI100000000&punto=indice"

$nombres=$web.AllElements | where class -eq "DifFlSb" | select -Skip 1
$nombres.innerText.ToLower()

$difs=$web.AllElements | where class -eq "DifClSb" | select -Skip 1
$difs.innerText.ToLower() | Out-File diferencias.txt

#Crear un worpres y sus respectivas carpetas y bases de datos para cada empresa
$linea = 2
foreach($nombrecito in $nombres.innerText.ToLower())
{
    $nombreprima="manu"+$nombrecito.replace(".","")
    $nombre=$nombreprima.replace(" ","")
    $nombremayus = $nombrecito.ToUpper()
    New-Item -ItemType Directory -Name www.$nombre.es -Force
    cd D:\xampp\htdocs\www.$nombre.es
    wp core download --locale=es_ES
    wp config create  --dbname=base_$nombre --dbuser=root --locale=es_ES
    wp db create
    wp core install --url=www.$nombre.es --title="Dominio $nombre" --admin_user=$nombre --admin_password=Andel1928 --admin_email=subida@email.com
    $contenido = " El valor de la bolsa de "+$nombre+" ha variado "+(Get-Content D:\xampp\htdocs\diferencias.txt | Select -Index ($linea - 1))
    wp post create --post_title="$nombremayus" --post_content="$contenido" --post_type="post" --post_status="publish"
    $linea++

    $hosts=
    "
    #$nombre
    127.0.0.1    www.$nombre.es
    "
    Add-Content C:\Windows\System32\drivers\etc\hosts -Value $hosts

    $vhost=
    "
     <VirtualHost *:80>
        ServerAdmin admin@www.$nombre.es
        DocumentRoot D:\\xampp\htdocs\www.$nombre.es
        ServerName www.$nombre.es
     </VirtualHost>
     "
    Add-Content D:\xampp\apache\conf\extra\httpd-vhosts.conf -Value $vhost

    cd ..
}


} '2' {       

#Sacar contenido de la web y meterlo en un archivo
cd D:\xampp\htdocs
$web = iwr "http://www.bolsamadrid.es/esp/aspx/Mercados/Precios.aspx?indice=ESI100000000&punto=indice"

$nombres=$web.AllElements | where class -eq "DifFlBj"
$nombres.innerText.ToLower()

$difs=$web.AllElements | where class -eq "DifClBj"
$difs.innerText.ToLower() | Out-File diferencias.txt

#Crear un worpres y sus respectivas carpetas y bases de datos para cada empresa
$linea = 2
foreach($nombrecito in $nombres.innerText.ToLower())
{
    $nombreprima="manu"+$nombrecito.replace(".","")
    $nombre=$nombreprima.replace(" ","")
    $nombremayus = $nombrecito.ToUpper()
    New-Item -ItemType Directory -Name www.$nombre.es -Force
    cd D:\xampp\htdocs\www.$nombre.es
    wp core download --locale=es_ES
    wp config create  --dbname=base_$nombre --dbuser=root --locale=es_ES
    wp db create
    wp core install --url=www.$nombre.es --title="Dominio $nombre" --admin_user=$nombre --admin_password=Andel1928 --admin_email=bajada@email.com
    $contenido = " El valor de la bolsa de "+$nombre+" ha variado "+(Get-Content D:\xampp\htdocs\diferencias.txt | Select -Index ($linea - 1))
    wp post create --post_title="$nombremayus" --post_content="$contenido" --post_type="post" --post_status="publish"
    $linea++

    $hosts=
    "
    #$nombre
    127.0.0.1    www.$nombre.es
    "
    Add-Content C:\Windows\System32\drivers\etc\hosts -Value $hosts

    $vhost=
    "
     <VirtualHost *:80>
        ServerAdmin admin@www.$nombre.es
        DocumentRoot D:\\xampp\htdocs\www.$nombre.es
        ServerName www.$nombre.es
     </VirtualHost>
     "
    Add-Content D:\xampp\apache\conf\extra\httpd-vhosts.conf -Value $vhost

    cd ..
}

} '3' {

#Sacar contenido de la web y meterlo en un archivo
cd D:\xampp\htdocs
$web = iwr "http://www.bolsamadrid.es/esp/aspx/Mercados/Precios.aspx?indice=ESI100000000&punto=indice"

$nombres=$web.AllElements | where class -eq "DifFlIg"
$nombres.innerText.ToLower()

$difs=$web.AllElements | where class -eq "DifClIg"
$difs.innerText.ToLower() | Out-File diferencias.txt

#Crear un worpres y sus respectivas carpetas y bases de datos para cada empresa
$linea = 2
foreach($nombrecito in $nombres.innerText.ToLower())
{
    $nombreprima="manu"+$nombrecito.replace(".","")
    $nombre=$nombreprima.replace(" ","")
    $nombremayus = $nombrecito.ToUpper()
    New-Item -ItemType Directory -Name www.$nombre.es -Force
    cd D:\xampp\htdocs\www.$nombre.es
    wp core download --locale=es_ES
    wp config create  --dbname=base_$nombre --dbuser=root --locale=es_ES
    wp db create
    wp core install --url=www.$nombre.es --title="Dominio $nombre" --admin_user=$nombre --admin_password=Andel1928 --admin_email=igual@email.com
    $contenido = " El valor de la bolsa de "+$nombre+" ha variado "+(Get-Content D:\xampp\htdocs\diferencias.txt | Select -Index ($linea - 1))
    $linea++
    wp post create --post_title="$nombremayus" --post_content="$contenido" --post_type="post" --post_status="publish"

    $hosts=
    "
    #$nombre
    127.0.0.1    www.$nombre.es
    "
    Add-Content C:\Windows\System32\drivers\etc\hosts -Value $hosts

    $vhost=
    "
     <VirtualHost *:80>
        ServerAdmin admin@www.$nombre.es
        DocumentRoot D:\\xampp\htdocs\www.$nombre.es
        ServerName www.$nombre.es
     </VirtualHost>
     "
    Add-Content D:\xampp\apache\conf\extra\httpd-vhosts.conf -Value $vhost

    cd ..
}

} '4' { 
                cls 
                return
           }
     }
     pause 
} 
until ($input -eq '4')
```
