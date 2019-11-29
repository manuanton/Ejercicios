#Conectarse a un equipo por SSH
New-SSHSession -ComputerName 192.168.110.2 -Credential manu

New-ADOrganizationalUnit -Name "UnidadPrueba" -ProtectedFromAccidentalDeletion $false -Path "DC=andel,DC=local"

#Antes de nada comprobar que el fichero no ha sido manipulado.
    #Sacar hash de los ficheros de texto en linux y windows y comprobar que son iguales
        #Linux
            $comando2=Invoke-SSHCommand -Index 0 "md5sum fichero.txt"
            $comprobarlinux=$comando2.Output
        #Windows
            del fichero.txt
            Get-SCPFile -LocalFile fichero.txt -RemoteFile fichero.txt -ComputerName 192.168.110.2 -Credential manu
            $comprobarwindows=Get-FileHash fichero.txt -Algorithm MD5

        #Comprobamos que sean iguales
            if($comprobarlinux.split(" ")[0] -eq $comprobarwindows.Hash){
            "Son iguales, puede continuar"
            } else {
            "Alerta! el fichero ha sido modficado"
            pause
            }


#Ahora, cumplir con lo que nos diga el fichero de linux de la siguiente forma:"orden"."dato"
foreach($linea in $comando1.Output)
{
    switch ($linea.split(".")[0])
    {
            'crearusuario'
                {
                    #Crear y meter el usuario en la OU
                        $contraseña="Andel1928"
                        New-ADUser -Name $linea.split(".")[1] -Sam $linea.split(".")[1] -Path "OU=UnidadPrueba,DC=andel,DC=local" -AccountPassword (ConvertTo-SecureString $contraseña -AsPlainText -force) -Enable $true 
                }
            'proceso' 
                {
                    #Ver información de los procesos y servicios
                        $proceso=$linea.split(".")[1]
                        Get-Process $proceso | Select-Object Name, Id, Company, Threads
                }
            'servicio' 
                {
                    #Ver información de los procesos y servicios
                        $servicio=$linea.split(".")[1]
                        Get-Service $servicio |  Select-Object Name, RequiredServices, Status, StartType
                }
    }
} pause
