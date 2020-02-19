# HackHealth-IBP
Creando una red de blockchain básica usando IBM Blockchain Platform.


# Introducción

Proyecto de como se creó la red de blockchain para el reto de [Hack Health 2020](https://hackathonspain.com/hackhealthbcn-2020/).
De como se desarrolló el [Smart Contract](https://hyperledger-fabric.readthedocs.io/en/release-1.4/smartcontract/smartcontract.html) y la aplicación base.


Dividiré el proyecto en 2 partes. La **Parte 1** consistirá en desplegar nuestra red usando  [IBM Cloud Kubernetes Service](https://www.ibm.com/cloud/container-service) y la consola de [IBM Blockchain Platform](https://www.ibm.com/cloud/blockchain-platform).
La **Parte 2** consistirá en el desarrollo de un [Smart Contract](https://hyperledger-fabric.readthedocs.io/en/release-1.4/smartcontract/smartcontract.html) y una aplicación utilizando [VSCode](https://code.visualstudio.com/)
y la extensión [IBM Blockchain Platform Extension for VSCode](https://marketplace.visualstudio.com/items?itemName=IBMBlockchain.ibm-blockchain-platform).

Al terminar sabrás:

* Desplegar tu propia red de blockchain funcional.

* Qué es un  [Smart Contract o Chaincode](https://hyperledger-fabric.readthedocs.io/en/release-1.4/smartcontract/smartcontract.html)  así cómo desarrollarlo, empaquetarlo e instalarlo en tu red.

* Invocar transacciones en la red desde una aplicación.





## Pasos para desplegar IBP

1. Crea tu servicio de IBM Cloud 
* Crea una cuenta en [IBM Cloud](https://cloud.ibm.com/registration?locale=es)
<p align="center">
  <img src="https://github.com/EmilioBerlanda/HackHealth-IBP/blob/master/Imagenes/ibm-cloud-registro.png">
</p>

2. Despliega el servicio de Kubernetes, escoge el plan gratuito, pónle un nombre y pincha crear. _Tarda unos 30 min_.
<p align="center">
  <img src="https://github.com/EmilioBerlanda/HackHealth-IBP/blob/master/Imagenes/kuber.gif">
</p>

3. Creamos el servicio de [IBM Blockchain Platform]()
<p align="center">
  <img src="https://github.com/EmilioBerlanda/HackHealth-IBP/blob/master/Imagenes/ibp.gif">
</p>

* Pinchamos en Lanzar y nos aparecera la IBM Platform 

<p align="center">
  <img src="https://github.com/EmilioBerlanda/HackHealth-IBP/blob/master/Imagenes/ibpplatform.gif">
</p>

## Pasos para construir la red de Blockchain

* **Importante**: La instalación de los elementos necesarios para montar la red es  **muy sencilla** con **IBP**. Dedícale un tiempo antes a pensar la arquitectura y cómo plasmar la realidad de la manera más funcional. _¿Qué organizaciones participan?, ¿qué actores existen en cada una de ellas y que roles adoptan?,_ **IBP** se basa en [Hyperledger Fabric](https://hyperledger-fabric.readthedocs.io/en/release-1.4/network/network.html) por lo que sería conveniente que dedicaras 15 minutos a leer como se organizan la redes.

<<<<<<< HEAD
## Construye la red.

* _Nota: Esta parte del tutorial es una traducción del tutorial de [Horea Porutiu](https://github.com/horeaporutiu). Os recomiendo encarecidamente que le echéis un vistazo a su cuenta de GitHub y a su [canal de Youtube](https://www.youtube.com/channel/UCSQpzlNDZOUR3LAaGWNMovw)._



* Counstriremos la red siguiendo la [documentacion](https://console.bluemix.net/docs/services/blockchain/howto/ibp-console-build-network.html#ibp-console-build-network) de IBM Blockchain Platform.  Esto incluirá crear un canal con un peer y un orderer cada uno con su propio MSP y CA [Certificate Authority](https://hyperledger-fabric.readthedocs.io/en/release-1.4/identity/identity.html#certificate-authorities). También crearemos sus respectivas identidades para desplegar los peers y operar con los nodos.

### Crea la primera organización y el punto de acceso a tu Blockchain

* #### Crea tu CA del peer
  - Click <b>Add Certificate Authority</b>.
  - Click <b>IBM Cloud</b> under <b>Create Certificate Authority</b> and <b>Next</b>.
  - Ponle un <b>Display name</b> of `Org1 CA`.  
  - Especifica un <b>Admin ID</b> of `admin` and <b>Admin Secret</b> of `adminpw`.




* #### Usa la CA para registrar identidades
  - Selecciona la  <b>Org 1 CA</b> Certificate Authority que has creado.
  - Primero, registraremos un admin para nuestra organización "org1". Pincha en el botón <b>Register User</b>.  Da un <b>Enroll ID</b> de `org1admin`, y <b>Enroll Secret</b> de `org1adminpw`.  Pincha <b>Next</b>.  Selecciona <b>Type</b> para esta identidad como  `client` y selecciona `org1` en la lista. Dejaremos los campos <b>Maximum enrollments</b> y <b>Add Attributes</b> en blanco.
  - Repetiremos este proceso para crear la identidad del peer.Pincha enel botón <b>Register User</b>. Da un  <b>Enroll ID</b> de `peer1`, y <b>Enroll Secret</b> de `peer1pw`. Pincha <b>Next</b>.  Selecciona el <b>Type</b> para esta identidad como `peer` y selecciona `org1`. Dejaremos los campos <b>Maximum enrollments</b> y <b>Add Attributes</b> en blanco.


* #### Crearemos la definicion del MSP de l organización
  - Navega hasta la pestaña <b>Organizations</b> y pincha <b>Create MSP definition</b>.
  - Enter the <b>MSP Display name</b> as `Org1 MSP` and an <b>MSP ID</b> of `org1msp`.
  - Under <b>Root Certificate Authority</b> details, specify the peer CA that we created `Org1 CA` as the root CA for the organization.
  - Give the <b>Enroll ID</b> and <b>Enroll secret</b> for your organization admin, `org1admin` and `org1adminpw`. Then, give the Identity name, `Org1 Admin`.
  - Click the <b>Generate</b> button to enroll this identity as the admin of your organization and export the identity to the wallet. Click <b>Export</b> to export the admin certificates to your file system. Finally click <b>Create MSP definition</b>.



* Create a peer
  - On the <b>Nodes</b> page, click <b>Add peer</b>.
  - Click <b>IBM Cloud</b> under Create a new peer and <b>Next</b>.
  - Give your peer a <b>Display name</b> of `Peer Org1`.
  - On the next screen, select `Org1 CA` as your <b>Certificate Authority</b>. Then, give the <b>Enroll ID</b> and <b>Enroll secret</b> for the peer identity that you created for your peer, `peer1`, and `peer1pw`. Then, select the <b>Administrator Certificate (from MSP)</b>, `Org1 MSP`, from the drop-down list and click <b>Next</b>.
  - Give the <b>TLS Enroll ID</b>, `admin`, and <b>TLS Enroll secret</b>, `adminpw`, the same values are the Enroll ID and Enroll secret that you gave when creating the CA.  Leave the <b>TLS CSR hostname</b> blank.
  - The last side panel will ask you to <b>Associate an identity</b> and make it the admin of your peer. Select your peer admin identity `Org1 Admin`.
  - Review the summary and click <b>Submit</b>.



### Create the node that orders transactions

* #### Create your orderer organization CA
  - Click <b>Add Certificate Authority</b>.
  - Click <b>IBM Cloud</b> under <b>Create Certificate Authority</b> and <b>Next</b>.
  - Give it a unique <b>Display name</b> of `Orderer CA`.  
  - Specify an <b>Admin ID</b> of `admin` and <b>Admin Secret</b> of `adminpw`.



* #### Use your CA to register orderer and orderer admin identities
  - In the <b>Nodes</b> tab, select the <b>Orderer CA</b> Certificate Authority that we created.
  - First, we will register an admin for our organization. Click on the <b>Register User</b> button.  Give an <b>Enroll ID</b> of `ordereradmin`, and <b>Enroll Secret</b> of `ordereradminpw`.  Click <b>Next</b>.  Set the <b>Type</b> for this identity as `client` and select `org1` from the affiliated organizations drop-down list. We will leave the <b>Maximum enrollments</b> and <b>Add Attributes</b> fields blank.
  - We will repeat the process to create an identity of the orderer. Click on the <b>Register User</b> button.  Give an <b>Enroll ID</b> of `orderer1`, and <b>Enroll Secret</b> of `orderer1pw`.  Click <b>Next</b>.  Set the <b>Type</b> for this identity as `peer` and select `org1` from the affiliated organizations drop-down list. We will leave the <b>Maximum enrollments</b> and <b>Add Attributes</b> fields blank.




* #### Create the orderer organization MSP definition
  - Navigate to the <b>Organizations</b> tab in the left navigation and click <b>Create MSP definition</b>.
  - Enter the <b>MSP Display name</b> as `Orderer MSP` and an <b>MSP ID</b> of `orderermsp`.
  - Under <b>Root Certificate Authority</b> details, specify the peer CA that we created `Orderer CA` as the root CA for the organization.
  - Give the <b>Enroll ID</b> and <b>Enroll secret</b> for your organization admin, `ordereradmin` and `ordereradminpw`. Then, give the <b>Identity name</b>, `Orderer Admin`.
  - Click the <b>Generate</b> button to enroll this identity as the admin of your organization and export the identity to the wallet. Click <b>Export</b> to export the admin certificates to your file system. Finally click <b>Create MSP definition</b>.


* #### Create an orderer
  - On the <b>Nodes</b> page, click <b>Add orderer</b>.
  - Click <b>IBM Cloud</b> and proceed with <b>Next</b>.
  - Give your peer a <b>Display name</b> of `Orderer`.
  - On the next screen, select `Orderer CA` as your <b>Certificate Authority</b>. Then, give the <b>Enroll ID</b> and <b>Enroll secret</b> for the peer identity that you created for your orderer, `orderer1`, and `orderer1pw`. Then, select the <b>Administrator Certificate (from MSP)</b>, `Orderer MSP`, from the drop-down list and click <b>Next</b>.
  - Give the <b>TLS Enroll ID</b>, `admin`, and <b>TLS Enroll secret</b>, `adminpw`, the same values are the Enroll ID and Enroll secret that you gave when creating the CA.  Leave the <b>TLS CSR hostname</b> blank.
  - The last side panel will ask to <b>Associate an identity</b> and make it the admin of your peer. Select your peer admin identity `Orderer Admin`.
  - Review the summary and click <b>Submit</b>.



* #### Add organization as Consortium Member on the orderer to transact
  - Navigate to the <b>Nodes</b> tab, and click on the <b>Orderer</b> that we created.
  - Under <b>Consortium Members</b>, click <b>Add organization</b>.
  - From the drop-down list, select `Org1 MSP`, as this is the MSP that represents the peer's organization org1.
  - Click <b>Submit</b>.



### Create and join channel

* #### Create the channel
  - Navigate to the <b>Channels</b> tab in the left navigation.
  - Click <b>Create channel</b>.
  - Give the channel a name, `mychannel`.
  - Select the orderer you created, `Orderer` from the orderers drop-down list.
  - Select the MSP identifying the organization of the channel creator from the drop-down list. This should be `Org1 MSP (org1msp)`.
  - Associate available identity as `Org1 Admin`.
  - Click <b>Add</b> next to your organization. Make your organization an <b>Operator</b>.
  - Click <b>Create</b>.

* #### Join your peer to the channel
  - Click <b>Join channel</b> to launch the side panels.
  - Select your `Orderer` and click <b>Next</b>.
  - Enter the name of the channel you just created. `mychannel` and click <b>Next</b>.
  - Select which peers you want to join the channel, click `Peer Org1` .
  - Click <b>Submit</b>.
=======
1. Crea
>>>>>>> a4d6f95dafd4584dca3cd2d5de9612f567a7ebfe



