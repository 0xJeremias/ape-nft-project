# Create your first NFT with ApeWorx & Vyper [[EN](#english)/[ES](#español)]
![Header](https://raw.githubusercontent.com/jbchoncen/ape-nft-project/master/img/head.png)
### English:
```
Hello! The objective of this tutorial is to share this step by step as if they were notes, so that you can understand what we are doing at all times and it is accessible to anyone without much knowledge.
```
The following guide is made following the challenge of [Apeworx Academy](https://academy.apeworx.io/challenge)

## Requirements for this tutorial:

- Python 3.7.2 or higher.
- Git
- Linux or mac OS.
- Windows install console [(WSL)](https://learn.microsoft.com/en-us/windows/wsl/install)
- VSCode
If you don't know if you have python installed and/or want to know what version of python you have use:
```
python3 –-version
```
If your python version is lower than 3.7.2, you can update it using (Tested on Fedora and Ubuntu):
```
sudo apt upgrade python3
```
Installing Git, you can use apt-get:
```
$ apt-get install git
```
## Preparing the environment:

We suggest always using virtual environments, to keep the computer clean, and easily reversible:

### Installing Venv:

Create your working folder, for example in my case:
```
/home/jere/project/apeworx/
```
In that folder create a virtual environment (where “ape” is the name we give to the virtual environment):
```
$ python3 -m venv ape
```

We activate the virtual environment that we created earlier "ape", in my case:

```
$ source ~/project/apeworx/ape/bin/activate
```
(on Spanish keyboard ~ is: "Alt Gr + ñ")

Ó is also valid (if you are stopped at /apeworx:
```
$ source ./ape/bin/activate
```

You should notice the prefix that appears on the command line, in my case:
```
(ape) jere@my:~/project/apeworx/
```
#### -Note, when you're done using the environment, you can remove it by simply using the word:

```
deactivate
```

## Installing Ape (via pip):

```
$ pip install -U pip
$ pip install eth-ape
```

-If you want to install via `pipx` or `Docker` visit the following [link](https://docs.apeworx.io/ape/stable/userguides/quickstart.html#installation).

Now you can check if it was installed correctly by querying the installed version:
```
$ ape --version
```

I should get a response, in my case:
```
0.5.1
```

## Installing Necessary Plugins:

Ape provides a lightweight installation with only necessary plugins, and gives you the ability to only install the ones you need,
thus avoid installing unnecessary software.

First we will install the following plugins:

```
$ ape plugins install template alchemy
```
Verify that you have Vyper installed:
```
$ ape plugins install vper -y
```

## Ape ERC721 plate:

```
$ ape template gh:ApeAcademy/ERC721
```

A menu will appear, in this case you could leave everything by default (it can be changed later), just pressing enter:

`project_name [ape-nft-project]:` ->(Enter to default to the name "ape-nft-project")
`token_name [Ape NFT]:` ->(Enter to default to "Ape NFT")
`token_symbol [APES]:` ->(Enter to default to "APES")
**And the rest all by default, enter, enter...**

We move inside the project folder:
```
$ cd ape-nft-project
```

We open with VSCode (with “code + space + period”):
`code .`.

After it starts, we open the terminal in VSCode with:
```
Ctrl + `
```

We activate the virtual environment again, in my case:

```
source ~/project/apeworx/ape-nft-project/ape/bin/activate
```

Or it is also valid (if you are stopped at /ape-nft-project):
```
source ../ape/bin/activate
```

With the following command, the missing plugins that the template needs are downloaded.
```
$ ape plugins install -U
```

If I get to this point I could see the template folders, and you can see the "NFT.vy" contract inside the "contracts" folder

I could run test and compile:
```
$ ape test
```
```
$ ape compile
```

If you followed the steps, you shouldn't have any errors.


- Congratulations for having reached this point!!, the next step is to prepare the image and the metadata for the NFT and for that we are going to use an external tool called PINATA:

## PINATA:

Enter the following link [Pinata](https://app.pinata.cloud/), and create an account, it's free.

There are different ways to work with Pinata, the simplest way is the following:

Attach a folder with the following name and structure:
```
folder
    ├──ape_bot_img
    | └── test_nft2.png
    └──ape_bot_meta
        └── 1
```

Download it and now in Pinata click on upload folder:

1)- Select the "ape_bot_img" folder, it will ask you with what name and write the same one that has "ape_bot_img".
- When uploaded copy the CID of the folder.

2)- Open with notepad the file "1" inside the folder "ape_bot_meta" and you will see the metadata file.
Search for "image", you have to paste in "YOUR_FOLDER_IMG_CID" the CID of the "ape_bot_img" folder that we just copied.

```
"image" : "https://ipfs.io/ipfs/YOUR_FOLDER_IMG_CID/test_nft2.png"
```
- Notice that we added the name of the image. "/test_nft2.png" must be at the end of the url.
  - If you want to know more about Metadata, I leave you this link: [eip-1155](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1155.md#erc-1155-metadata-uri-json-schema), but I recommend that you experiment outside of the tutorial, to avoid errors.

- The file "1" save it without extension.
- For this course we will make 10 NFT. For that, copy and paste in the same folder the file "1" and only change the names,
 Name them from 1 to 10.

3)- In Pinata upload the folder "ape_bot_meta" and give it the same name.
 - Copy the CID from the "ape_bot_meta" folder

## Customizing the template:

1)- Open "NFT.vy" inside the "contracts" folder and let's modify some things:

look for the lines:

1)-
```
MAX_SUPPLY: constant(uint256) = 10000
```
This is the maximum amount to issue, and we will only do 10, change it.

2)-
```
self.baseURI = "https://ipfs.io/ipfs/YOUR_FOLDER_META_CID"
```
Put the CID of the folder "ape_bot_meta"

3)-

```
assert nft.baseURI() == "https://ipfs.io/ipfs/YOUR_FOLDER_META_CID"
    nft.mint(owner, sender=owner)
assert nft.tokenURI(1) == "https://ipfs.io/ipfs/YOUR_FOLDER_META_CID/1"
```
Modify these two URLs also with the same CID in the folder "tests/test_token.py"


# Connecting to Goerli via Alchemy:

To be able to connect to the Ethereum network it is necessary to have a node, luckily there are services that facilitate this work, for that we are going to register in [Alchemy](https://www.alchemy.com/).

Currently, when we register Alchemy automatically configures us a Demo account in Goerli, we can use it without problem.

1)-
We click on `"View Key"` and then copy the `"API KEY"`

One of the many great things about ApeWorx is that this "API KEY" is stored in local environment variables, making production work more secure.

To save the Alchemy API KEY:
In terminal run:
(Linux/Windows WSL)

```
nano ~/.bashrc
```
 either
(Macs)
```
nano ~/.zshrc
```

Add the line:
```
export WEB3_ALCHEMY_PROJECT_ID=YOUR_ALCHEMY_API_KEY
```
Put the `API-KEY` we got from Alchemy
Save the changes:

`Crtl + x`, asks if we want to save the changes, press `Y` and Enter.

With the following command we refresh the environment variables:
```
source ~/.bashrc
```
To confirm that we loaded the `API-KEY` correctly, we execute the following command and it should show us the `API-KEY`.
```
echo $WEB3_ALCHEMY_PROJECT_ID
```

# Create or import a Wallet Address:


## To create:

```
$ ape accounts generate "test"
$ Add extra entropy for key generation...:
$ Create Passphrase:
$ Repeat for confirmation:
$ SUCCESS: A new account '0x157h15An3XAMpL30fANwall3tADdr355H3ll07h3r3' has been added with the id 'test'
```
"test": is the personalized name we give it to identify it among our accounts.
Entropy: here you can put anything like for example: asdjkldsfjoaif
Password and repeat.
Then confirm that the creation was successful, with the address and ID


## To import:
```
$ ape accounts import "test_imported"
$ Enter Private Key:
$ Create Passphrase:
$ Repeat for confirmation:
```
In this case the only difference is that the "private key" must be entered, which for example in Metamask can be found in:
(...) , Accounts Details, Export private Key, (you will be asked for your password).


# Anchoring:
You can fund your account at the following [link](https://goerlifaucet.com/). Enter the address of your account and in a few seconds you will have test ether.

You can list the accounts registered with:
```
ape accounts list --all
```

# Connect to Goerli and implement:

Before connecting, you could run the following command to display the connection structure:
```
ape networks list
```

Now yes, to connect to Goerli:
```
ape console --network ethereum:goerli:alchemy
```
This will return an interactive python console.
Now we can check the balance to see if the funds have arrived:

```
test01 = accounts.load("test")
```
What we are doing is loading the "test" account that we already created from accounts and we save it in the variable "test01"

We check the balance:
```
test01.balance
```
This will return our balance, but if we want to see it in a more friendly format, we better execute:
```
test01.balance / 1e18
```

# Deploying:

```
contract = project.NFT.deploy(sender=test01)
```
Here we are saying that we are going to deploy the project/contract called "NFT" and that the address that signs it is ours, that of the creator, the one that we previously saved in the variable "test01" and save it in the variable "contract"

Asks us:
- If we want to sign: Yes
- Let us enter the password
- If we leave the account unlocked to continue working with it: Yes
It returns a lot of data, including the "address of the contract".

and Ready!!, But still the NFT do not exist, for that you have to do mint:

```
contract.mint(test01, sender=test01)
```
With this line we are telling it that we are going to mint the contract towards our own account, the first `test01` is the recipient and it goes between "", but here it is a variable, for example:

If you wanted to send your NFT to the people at ApeWorx, the format would be as follows:

```
contract.mint("0x187089b65520D2208aB93FB471C4970c29eAf929", sender=test01)
```
You can now go to the [OpenSea Testnet](https://testnets.opensea.io/), paste your address into the search box and click your mouse to see your NFTs.

If in the future you want to reconnect to mint the remaining NFTs, you don't have to dploy again, you can connect to the contract with the following command from "ape console":
```
my_contract = project.NFT.at(contract_address)
```
and then:
```
contract.mint("destination_address", sender=test01)
```


That's all, congratulations!! I hope it has been helpful!
Once you have reached this point, you have a base to continue exploring, you can try more things, ApeWorx has a lot of documentation and new things to explore, below I leave the links to continue exploring:

[ApeWorx Academy](https://academy.apeworx.io/)  
[ApeWorx Docs](https://docs.apeworx.io/ape/stable/index.html)  
[Vyper Docs](https://vyper.readthedocs.io/en/stable/)  
[Apeworx Github](https://github.com/ApeWorX)  
[Ape Academy GitHub](https://github.com/ApeAcademy)  
[ApeWorx Youtube](https://www.youtube.com/channel/UCz-cD1gjJnSeP_Z0Bu1h05A)  


___________



### Español:
```
Hola! el objetivo de este tutorial, es el de compartir este paso a paso como si fueran apuntes, para que pueda entender que estamos haciendo en cada momento y sea accesible a cualquier persona sin mucho conocimiento.
```

La siguiente guia esta realizada siguiendo el challenge de [Apeworx Academy](https://academy.apeworx.io/challenge)

## Requisitos para este tutorial:

-   Python 3.7.2 o superior.
-   Git
-   Linux o mac OS.
-   Windows instalar la consola [(WSL)](https://learn.microsoft.com/en-us/windows/wsl/install)
-   VSCode

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
Verifique que tiene instalado Vyper:  
```
$ ape plugins install vyper -y
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

Podria ejectuar test y compilar:
```
$ ape test
```
```
$ ape compile
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
    └──ape_bot_meta  
        └── 1  
```

Descarguela y ahora en Pinata haga click en upload folder: 

1)- Seleccione la carpeta "ape_bot_img", le va a preguntar con que nombre y escriba el mismo que tiene "ape_bot_img".
- Cuando se suba copie el CID de la carpeta.

2)- Abra con el block de notas el archivo "1" dentro de la carpeta "ape_bot_meta" y verá el archivo de metadatos.
Busque "image", tiene que pegar en "YOUR_FOLDER_IMG_CID" el CID de la carpeta "ape_bot_img" que recien copiamos.

```
"image" : "https://ipfs.io/ipfs/YOUR_FOLDER_IMG_CID/test_nft2.png"
```
- Note que agregamos el nombre de la imagen. "/test_nft2.png" debe quedar al final de la url.
  - Si quiere saber mas de Metadata, le dejo este link: [eip-1155](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1155.md#erc-1155-metadata-uri-json-schema), pero recomiendo que experimente fuera del tutorial, para evitar errores.

- El archivo "1" guardelo sin extension.
- Para este curso haremos 10 NFT. Para eso, copie y pegue en la misma carpeta el archivo "1" y solo cambie los nombres,
 nombrelos de 1 a 10.

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
$ SUCCESS: A new account '0x157h15An3XAMpL30fANwall3tADdr355H3ll07h3r3' has been added with the id 'test' 
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
Aqui estamos diciendo que vamos a hacer el deploy del proyecto/contrato llamado "NFT" y que la direccion que lo firma es la nuestra la del creador, la que guardamos previamente en la variable "test01" y guardarlo en la variable "contract"

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
Una vez que haya llegado a este punto tiene una base para luego seguir explorando puede ir probando mas cosas, ApeWorx tiene mucha documentacion y cosas nuevas por explorar a continuacion dejo los links para seguir profundizando:

[ApeWorx Academy](https://academy.apeworx.io/)  
[ApeWorx Docs](https://docs.apeworx.io/ape/stable/index.html)  
[Vyper Docs](https://vyper.readthedocs.io/en/stable/)  
[Apeworx Github](https://github.com/ApeWorX)  
[Ape Academy GitHub](https://github.com/ApeAcademy)  
[ApeWorx Youtube](https://www.youtube.com/channel/UCz-cD1gjJnSeP_Z0Bu1h05A)  

