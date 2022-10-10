# Creando tu primer NFT con ApeWorx & Vyper

## Requisitos para este tutorial:


- Python 3.7.2 o superior.
- Linux o mac OS.
- Windows instalar [(WSL)](https://learn.microsoft.com/en-us/windows/wsl/install)
- VSCode (por ser el más popular)


Si no sabe si tiene python instalado y/o quiere saber que version de python tiene use:  
`python3 –-version`.

Si su versión de python es menor a 3.7.2, puede actualizarla utilizando (Probé en Fedora y Ubuntu):  
`sudo apt upgrade python3`.

## Preparando el entorno:

Sugerimos utilizar siempre entornos virtuales, para mantener la computadora limpia, y facilmente reversible:

### Instalando Venv:

Cree su carpeta de trabajo, por ejemplo en mi caso:  
`/home/jere/project/apeworx/`.

En esa carpeta crear un entorno virtual (donde “ape” es el nombre que le damos al entorno virtual):  
`python3 -m venv ape`.

Ahora creamos una carpeta para el nombre específico del proyecto y luego nos movemos dentro:

```
mkdir ape-nft-project

cd ape-nft-project
```

Ahora la abrimos con VSCode (con “code + espacio + punto”):
`code .`.

Después que abra VSCode, abrimos la terminal con:
`Ctrl + ` `.

Ahora activamos el entorno virtual que creamos anteriormente “ape”, en mi caso:

`source ~/project/apeworx/ape-nft-project/ape/bin/activate`.
(en teclado español ~ es: "Alt Gr + ñ")

Ó también es válido (si te encuentras parado en /ape-nft-project):  
`source ../ape/bin/activate`.

Debería notar que esta dentro y haciendo uso del espacio virtual gracias al prefijo que le aparece en la línea de comandos, en mi caso:  
`(ape) jere@my:~/project/apeworx/ape-nft-project`.

##### -Nota, cuando termines de usar el entorno, puedes quitarlo usando simplemente la palabra:

`deactivate`

## Instalando Ape (via pip):

```
pip install -U pip
pip install eth-ape
```

-Si desea instalar via `pipx` o `Docker` visite el siguiente [link](https://docs.apeworx.io/ape/stable/userguides/quickstart.html#installation).

Ahora puede chequear si se instalo correctamente consultando la version instalada:  
`ape --version`

Deberia obtener una respuesta, en mi caso:  
`0.5.1`

## Instalando Plugins necesarios:

Ape instala una instalacion liviana con herramientas necesarias, y deja que usted instale luego las que necesite,
asi evitar que usted instale software inecesario.
Para este proyecto instalaremos los siguientes plugins:

```
pip install ape-ens

```
