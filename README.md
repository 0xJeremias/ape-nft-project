# Creando tu primer NFT con ApeWorx & Vyper

## Requisitos para este tutorial:

-   Python 3.7.2 o superior.
-   Git
-   Linux o mac OS.
-   Windows instalar [(WSL)](https://learn.microsoft.com/en-us/windows/wsl/install)
-   VSCode (por ser el más popular)

Si no sabe si tiene python instalado y/o quiere saber que version de python tiene use:  
`python3 –-version`.

Si su versión de python es menor a 3.7.2, puede actualizarla utilizando (Probado en Fedora y Ubuntu):  
`sudo apt upgrade python3`.

Instalando Git, puedes usar apt-get:

`$ apt-get install git`.

## Preparando el entorno:

Sugerimos utilizar siempre entornos virtuales, para mantener la computadora limpia, y facilmente reversible:

### Instalando Venv:

Cree su carpeta de trabajo, por ejemplo en mi caso:  
`/home/jere/project/apeworx/`.

En esa carpeta crear un entorno virtual (donde “ape” es el nombre que le damos al entorno virtual):  
`$ python3 -m venv ape`.

Activamos el entorno virtual que creamos anteriormente “ape”, en mi caso:

`$ source ~/project/apeworx/ape/bin/activate`.
(en teclado español ~ es: "Alt Gr + ñ")

Ó también es válido (si te encuentras parado en /apeworx:  
`$ source ./ape/bin/activate`.

Debería notar que esta dentro y haciendo uso del espacio virtual gracias al prefijo que le aparece en la línea de comandos, en mi caso:  
`(ape) jere@my:~/project/apeworx/`.

##### -Nota, cuando termines de usar el entorno, puedes quitarlo usando simplemente la palabra:

`deactivate`.

## Instalando Ape (via pip):

```
$ pip install -U pip
$ pip install eth-ape
```

-Si desea instalar via `pipx` o `Docker` visite el siguiente [link](https://docs.apeworx.io/ape/stable/userguides/quickstart.html#installation).

Ahora puede chequear si se instalo correctamente consultando la version instalada:  
`$ ape --version`

Deberia obtener una respuesta, en mi caso:  
`0.5.1`

## Instalando Plugins necesarios:

Ape proporciona una instalacion liviana solo con plugins necesarias, y le dal la posibilidad de solo instalar las que necesite,
asi evitar instalar software innecesario.

Primero instalaremos los siguientes plugins:

```
$ ape plugins install template alchemy
```

## Ape ERC721 emplate:

```
$ ape template gh:ApeAcademy/ERC721
```

Saldra un menu, para este caso podria dejar todo por default (mas adelante se peude cambiar), solo aprentando enter:

`project_name [ape-nft-project]:` ->(Enter para aceptar por default el nombre "ape-nft-project")  
`token_name [Ape NFT]:` ->(Enter para dejarlo por default "Ape NFT")  
`token_symbol [APES]:` ->(Enter para dejarlo por default "APES")  
**Y el resto todo por default, enter, enter..**

Nos movemos dentro de la carpeta del proyecto:

```
$ cd ape-nft-project
```

Abrimos con VSCode (con “code + espacio + punto”):
`code .`.

Después que inicie, abrimos la terminal en VSCode con:
`Ctrl + ` `.

Activamos el entorno virtual nuevamente, en mi caso:

`source ~/project/apeworx/ape-nft-project/ape/bin/activate`.

Ó también es válido (si te encuentras parado en /ape-nft-project):  
`source ../ape/bin/activate`.

Con el siguiente comando se terminan de bajar plugins faltantes que necesite el template.
`$ ape plugins install -U `

Si llego a este punto podria ver las carpetas del template, y puede visualizar dentro de la carpeta "contracts" el contrato "NFT.vy"

Podria ejectuar:
`$ ape test`

Y no deberia tener ningun error.
Felicitaciones por haber llegado hasta este punto!!, el siguiente paso es preparar el NFT y para eso vamos a utilizar una herramienta externa llamada PINATA:
