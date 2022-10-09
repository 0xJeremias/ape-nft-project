#Creando tu primer NFT con ApeWorx & Vyper

Requisitos para este tutorial:
VSCode (por ser el más popular)
Python 3.7.2 o superior.
Linux o mac OS.
Windows instalar (WSL)
VSCode (por ser el más popular)

Si no sabe si tiene python instalado y/o quiere saber que version de python tiene use:
python3 –version
Si su versión de python es menor a 3.7.2, puede actualizarla utilizando (Probé en Fedora y Ubuntu):
sudo apt upgrade python3

Instalando Ape:
Sugerimos utilizar siempre entornos virtuales, para mantener la computadora limpia:
instalando Venv
Cree su carpeta de trabajo, por ejemplo en mi caso:
/home/jere/project/apeworx/
En esa carpeta crear un entorno virtual (donde “ape” es el nombre que le damos al entorno virtual):
python3 -m venv ape
Ahora creamos una carpeta para el nombre específico del proyecto, en mi caso:
/home/jere/project/apeworx/ape-nft-project/
Ahora nos corremos a esa carpeta con “change directory”:
cd ape-nft-project
Ahora la abrimos con VSCode (con “code + espacio + punto”):
code .
Después que abra VSCode, abrimos la terminal con:
Ctrl + `
Ahora activamos el entorno virtual que creamos anteriormente “ape”, en mi caso:
source ~/project/apeworx/ape-nft-project/ape/bin/activate
También es válido (si te encuentras parado en /ape-nft-projec ):
source ../ape/bin/activate
Debería notar que esta dentro y haciendo uso del espacio virtual gracias a que le aparece en la línea de comandos, en mi caso:
(ape) jere@my:~/project/apeworx/ape-nft-project$
Nota, cuando termines de usar el entorno, puedes quitarlo usando simplemente la palabra:
deactivate
