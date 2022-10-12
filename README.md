# Creando tu primer NFT con ApeWorx (Template) & Vyper
![Header](https://raw.githubusercontent.com/jbchoncen/ape-nft-project/master/img/head.png)


## Requisitos para este tutorial:

-   Python 3.7.2 o superior.
-   Git
-   Linux o mac OS.
-   Windows instalar [(WSL)](https://learn.microsoft.com/en-us/windows/wsl/install)
-   VSCode (por ser el más popular)

Si no sabe si tiene python instalado y/o quiere saber que version de python tiene use:  
```
python3 –-version
```

Si su versión de python es menor a 3.7.2, puede actualizarla utilizando (Probado en Fedora y Ubuntu):  
```
sudo apt upgrade python3
```

Instalando Git, puedes usar apt-get:

```
$ apt-get install git
```

## Preparando el entorno:

Sugerimos utilizar siempre entornos virtuales, para mantener la computadora limpia, y fácilmente reversible:

### Instalando Venv:

Cree su carpeta de trabajo, por ejemplo en mi caso:  
```
/home/jere/project/apeworx/
```

En esa carpeta crear un entorno virtual (donde “ape” es el nombre que le damos al entorno virtual):  
```
$ python3 -m venv ape
```

Activamos el entorno virtual que creamos anteriormente “ape”, en mi caso:

```
$ source ~/project/apeworx/ape/bin/activate
```
(en teclado español ~ es: "Alt Gr + ñ")

Ó también es válido (si te encuentras parado en /apeworx:  
```
$ source ./ape/bin/activate
```

Debería notar que esta dentro y haciendo uso del espacio virtual gracias al prefijo que le aparece en la línea de comandos, en mi caso:  
```
(ape) jere@my:~/project/apeworx/
```

#### -Nota, cuando termines de usar el entorno, puedes quitarlo usando simplemente la palabra:

```
deactivate
```

## Instalando Ape (via pip):

```
$ pip install -U pip
$ pip install eth-ape
```

-Si desea instalar via `pipx` o `Docker` visite el siguiente [link](https://docs.apeworx.io/ape/stable/userguides/quickstart.html#installation).

Ahora puede chequear si se instalo correctamente consultando la version instalada:  
```
$ ape --version
```

Deberia obtener una respuesta, en mi caso:  
```
0.5.1
```

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
```
Ctrl + ` 
```

Activamos el entorno virtual nuevamente, en mi caso:

```
source ~/project/apeworx/ape-nft-project/ape/bin/activate
```

Ó también es válido (si te encuentras parado en /ape-nft-project):  
```
source ../ape/bin/activate
```

Con el siguiente comando se terminan de bajar plugins faltantes que necesite el template.
```
$ ape plugins install -U 
```

Si llego a este punto podria ver las carpetas del template, y puede visualizar dentro de la carpeta "contracts" el contrato "NFT.vy"

Podria ejectuar:
```
$ ape test
```

Si siguió los pasos, no deberia tener ningun error.


-Felicitaciones por haber llegado hasta este punto!!, el siguiente paso es preparar la imagen y la metadata para el NFT y para eso vamos a utilizar una herramienta externa llamada PINATA:

## PINATA:

Ingrese al siguiente link [Pinata](https://app.pinata.cloud/), y cree una cuenta, es gratis.

Hay diferentes maneras de trabajar con Pinata, la manera mas simple es la siguiente:

Adjunte una carpeta con el siguiente nombre y estructura:
```
folder  
    ├──ape_bot_img  
    |   └── test_nft2.png  
    ├──ape_bot_meta  
        └── 1  
```

Descarguela y ahora en Pinata haga click en upload folder: 

1)- Seleccione la carpeta "ape_bot_img", le va a preguntar con que nombre y escriba el mismo que tiene "ape_bot_img".
- Cuando se suba copie el CID de la carpeta.

2)- Abra con el block de notas el archivo "1" dentro de la carpeta "ape_bpt_meta" y verá el archivo de metadatos.
Busque "image", tiene que pegar en "YOUR_FOLDER_IMG_CID" el CID de la carpeta "ape_bot_img" que recien copiamos.

```
"image" : "https://ipfs.io/ipfs/YOUR_FOLDER_IMG_CID/test_nft2.png"
```
- Note que agregamos el nombre de la imagen. "/test_nft2.png" debe quedar al final de la url.

- El archivo "1" guardelo sin extension.
- Para este curso haremos 10 NFT. Para eso, copie y pegue en la misma carpeta el archivo "1" y nombrelos de 1 a 10.

3)- En Pinata suba la carpeta "ape_bot_meta" y asignele el mismo nombre.
 - Copie el CID de la carpeta "ape_bot_meta"

## Personalizando el template:

1)- Abra "NFT.vy" dentro de la carpeta "contracts" y modifiquemos algunas cosas:

busque las lineas:

1)-
```
MAX_SUPPLY: constant(uint256) = 10000
```
Esta es la cantidad maxima a emitir, y haremos solo 10, cambielo.

2)-
```
self.baseURI = "https://ipfs.io/ipfs/YOUR_FOLDER_META_CID"
```
Ponga el CID de la carpeta "ape_bot_meta"

3)-

```
assert nft.baseURI() == "https://ipfs.io/ipfs/YOUR_FOLDER_META_CID"
    nft.mint(owner, sender=owner)
assert nft.tokenURI(1) == "https://ipfs.io/ipfs/YOUR_FOLDER_META_CID/1"       
```
Modifique estas dos URL tambien con el mismo CID en la carpeta "tests/test_token.py"


# Conectándose a Goerli via Alchemy:

Para poder conectarnos a la red de Ethereum es necesario de diponer de un nodo, por suerte hay servicios que nos facilitan ese trabajo, para eso nos vamos a registrar en [Alchemy](https://www.alchemy.com/).

Actualemente, cuando nos registramos Alchemy nos configura automaticamente una cuenta Demo en Goerli, podemos usar esta sin problema.

1)-  
Hacemos click en `"View Key"` y luego copiamos el `"API KEY"`

Una de las tantas cosas fabulosas que tiene ApeWorx es que esta "API KEY" queda almacenada en las variables de entorno local, haciendo mas seguro el trabajo en produccion.

Para guardar la API KEY de Alchemy: 
En terminal ejecutar:   
(Linux/Windows WSL)

```
nano ~/.bashrc 
```
 ó  
(Mac)
```
nano ~/.zshrc 
```  

Agregar la linea:
```
export WEB3_ALCHEMY_PROJECT_ID=YOUR_ALCHEMY_API_KEY
```
Poner la `API-KEY` que obtuvimos de Alchemy
Salvar los cambios:

`Crtl + x`, pregunta si queremos guardar los cambios pulsamos `Y` y Enter.

Con el siguiente comando refrescamos las variables de entorno:
```
source ~/.bashrc
```
Para confirmar que cargamos bien la `API-KEY`, ejecutamos el siguiente comando y nos tendria que mostrar la `API-KEY`.
```
echo $WEB3_ALCHEMY_PROJECT_ID
```

# Crear o importar una Wallet Address:


## Crear:

```
$ ape accounts generate "test"
$ Add extra entropy for key generation...:
$ Create Passphrase:
$ Repeat for confirmation:
$ SUCCESS: A new account '0x1234567890ABCDEFGHiJKlmNOPQRSTUV2WXYz123456' has been added with the id 'test'
```
"test": es el nombre personalizado que le damos para identificarla entre nuestras cuentas.  
Entropy: aca puede poner cualquier cosa como por ejemplo: asdjkldsfjoaif      
Contraseña y repetir.  
Luego confirma que la creacion fue satisfactoria, con la direccion y el ID  


## Importar:
```
$ ape accounts import "test_imported"
$ Enter Private Key:
$ Create Passphrase:
$ Repeat for confirmation:
```
En este caso la unica diferencia es que se debe ingresar la "private key", que por ejemplo en Metamask la puede encontrar en:
(...) , Accounts Details, Export private Key, (se le pedira su password).


# Fondeando:
Puede fondear su cuenta en el siguiente [link](https://goerlifaucet.com/). Ingresa la direccion de su cuenta y en pocos segundos tendra ether de prueba.

Puede listar las cuentas registradas con:
```
ape accounts list --all
```

# Conectarse a Goerli e implementar:

Antes de conectar, podria ejecutar el siguiente comando para visualizar la estructura de coneccion:
```
ape networks list
```

Ahora sí, para conectar a Goerli:
```
ape console --network ethereum:goerli:alchemy
```
Esto nos devolvera una consola intertactiva de python.
Ahora podremos consultar el balance para ver si nos llegaron los fondos:

```
test01 = accounts.load("test")
```
Lo que estamos haciendo, es cargar la cuenta "test" que ya creamos desde accounts y la guardamos en la variable "test01"

Consultamos el balance:
```
test01.balance
```
Esto nos devolvera nuestro balance, pero si queremos verlo en un formato mas amigable mejor ejecutemos:
```
test01.balance / 1e18
```

# Deploy / implementar:

```
contract = project.NFT.deploy(sender=test01)
```
Aqui estamos diciendo que vamos a hacer el deploy del proyecto/contrato llamado "NFT" y que la cuenta que lo firma es la nuestra, la guardada en la variable "test01" y guardarlo en la variable "contract"

Nos pregunta:
- Si queremos firmar: Sí
- Que ingresemos el password
- Si dejamos la cuenta desbloqueada para seguir trabajando con ella : Sí   
Nos devuelve muchos datos, entre ellos la "direccion del contrato".

y Listoooo!!, Pero aún los NFT no existen, para eso hay que hacer mint:

```
contract.mint(test01, sender=test01)
```
Con esta linea le estamos diciendo vamos a hacer mint del contrato hacia nuestra misma cuenta, el primer `test01` es el destinatario y va entre "", pero aqui es una variable, por ejemplo:

Si quisiera enviarle su NFT a la gente de ApeWorx, el formato seria el siguiente:

```
contract.mint("0x187089b65520D2208aB93FB471C4970c29eAf929", sender=test01)
```
Ahora puede dirigirse a la Testnet de [OpenSea](https://testnets.opensea.io/), pegar su direccion en el buscador y hacer click con el mouse para ver sus NFT.

Si en un futuro quiere volver a conectarse para hacer mint de los restantes NFT, no tiene que hacer dploy nuevamente, puede conectarse al contrato con la siguiente comando desde "ape console":
```
my_contract = project.NFT.at(direccion_del_contrato)
```
y luego:   
```
contract.mint("direccion_destino", sender=test01)
```


Eso es todo, felicitaciones espero que le haya sido de ayuda!.
Una vez que haya llegado a este punto tiene una base para luego seguir explorando puede ir probando mas cosas, ApeWorx tiene mucha documentacion y cosas nuevas por probar a continuacion dejo los links para seguir profundizando:

[ApeWorx Academy](https://academy.apeworx.io/)  
[ApeWorx Docs](https://docs.apeworx.io/ape/stable/index.html)  
[Vyper Docs](https://vyper.readthedocs.io/en/stable/)  
[Apeworx Github](https://github.com/ApeWorX)  
[Ape Academy GitHub](https://github.com/ApeAcademy)  
[ApeWorx Youtube](https://www.youtube.com/channel/UCz-cD1gjJnSeP_Z0Bu1h05A)  

