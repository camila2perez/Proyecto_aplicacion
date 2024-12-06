# Configuración de RA (Fundación Unity con AR)

El siguiente script detecta superficies planas y coloca una obra de arte (modelo 3D o imagen 2D) en el entorno del usuario:

### codigo 

     using UnityEngine;
     using UnityEngine.XR.ARFoundation;
     using UnityEngine.XR.ARSubsystems;

    public class ARPlacementManager : MonoBehaviour
    {
    public GameObject artworkPrefab; // Prefab del modelo de arte
    private ARRaycastManager raycastManager;
    private List<ARRaycastHit> hits = new List<ARRaycastHit>();

    void Start()
    {
        raycastManager = GetComponent<ARRaycastManager>();
    }

    void Update()
    {
        // Detecta superficies planas con el toque del usuario
        if (Input.touchCount > 0)
        {
            Touch touch = Input.GetTouch(0);
            if (touch.phase == TouchPhase.Began)
            {
                if (raycastManager.Raycast(touch.position, hits, TrackableType.PlaneWithinBounds))
                {
                    Pose hitPose = hits[0].pose;
                    Instantiate(artworkPrefab, hitPose.position, hitPose.rotation);
                }
            }
        }
    }
    }

#  Personalización de Obras de Arte

Para cambiar el color o el marco en tiempo real, usamos shaders y materiales dinámicos:

### Codigo

    using UnityEngine;

    public class ArtworkCustomizer : MonoBehaviour
    { 
    public Material artworkMaterial; // Material del arte

    public void ChangeColor(Color newColor)
    {
        artworkMaterial.color = newColor;
    }

    public void ChangeFrameStyle(Texture newFrameTexture)
    {
        artworkMaterial.SetTexture("_MainTex", newFrameTexture);
    }
    }

# Guardar datos en Firebase

La configuración personalizada se guarda en Firebase para que el usuario pueda recuperarla Para guardar configuraciones personalizadas en Firebase Realtime Database , utilice un script en Unity con el SDK de Firebase:

Configuración en Firebase:

Configura un proyecto en Firebase Console .

Habilita Realtime Database y agrega las credenciales del proyecto a Unity.

### Código para guardar datos personalizados:

    using Firebase.Database;
    using UnityEngine;

    public class FirebaseManager : MonoBehaviour
    {
    private DatabaseReference dbReference;

    void Start()
    {
        // Inicializa la referencia a la base de datos
        dbReference = FirebaseDatabase.DefaultInstance.RootReference;
    }

    public void SaveCustomization(string userId, string artworkId, string colorHex, string frameStyle)
    {
        // Crear un objeto de datos personalizado
        ArtworkData artworkData = new ArtworkData(colorHex, frameStyle);
        
        // Guardar los datos en Firebase bajo el ID del usuario y la obra
        string jsonData = JsonUtility.ToJson(artworkData);
        dbReference.Child("users").Child(userId).Child("customizations").Child(artworkId).SetRawJsonValueAsync(jsonData)
            .ContinueWith(task => 
            {
                if (task.IsCompleted)
                    Debug.Log("Datos guardados correctamente en Firebase.");
                else
                    Debug.LogError("Error al guardar en Firebase: " + task.Exception);
            });
    }
    }

     // Clase para representar los datos personalizados
     [System.Serializable]
     public class ArtworkData
       
    {
    public string colorHex;
    public string frameStyle;

    public ArtworkData(string colorHex, string frameStyle)
    {
        this.colorHex = colorHex;
        this.frameStyle = frameStyle;
    }
    }

 # Acuñación de NFT con web3.js

Para acuñar el NFT, necesitas integrar la blockchain Ethereum y un contrato inteligente que administre los NFT. Aquí usaremos web3.js para interactuar con un contrato en un backend Node.js.

Contrato inteligente en Solidity:

Este contrato básico permite acuñar NFT personalizados:

    // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.0;

    import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
    import "@openzeppelin/contracts/access/Ownable.sol";

    contract ArtworkNFT is ERC721URIStorage, Ownable {
    uint256 public tokenCounter;

    constructor() ERC721("ArtworkNFT", "ARTNFT") {
        tokenCounter = 0;
    }

    function mintNFT(address recipient, string memory tokenURI) public onlyOwner returns (uint256) {
        uint256 newItemId = tokenCounter;
        _safeMint(recipient, newItemId);
        _setTokenURI(newItemId, tokenURI);
        tokenCounter++;
        return newItemId;
    }
    }
Backend Node.js con web3.js:

Este código interactúa con el contrato inteligente para acuñar un NFT.

Instale web3.js y configure el backend:

    npm install web3 dotenv

### Código para acuñar el NFT:

    const Web3 = require('web3');
    const contractABI = require('./ArtworkNFTABI.json');
    const contractAddress = "0xYourContractAddress";
    const web3 = new Web3("https://mainnet.infura.io/v3/YourProjectID");

    const account = "0xYourAccountAddress";
    const privateKey = "YourPrivateKey";

    const mintNFT = async (recipient, tokenURI) => {
    const contract = new web3.eth.Contract(contractABI, contractAddress);
    const tx = {
        from: account,
        to: contractAddress,
        data: contract.methods.mintNFT(recipient, tokenURI).encodeABI(),
        gas: 2000000,
    };

    const signedTx = await web3.eth.accounts.signTransaction(tx, privateKey);
    const receipt = await web3.eth.sendSignedTransaction(signedTx.rawTransaction);
    console.log("Transaction successful: ", receipt.transactionHash);
    };

    // Llamar a la función para acuñar
    mintNFT("0xRecipientAddress", "https://ipfs.io/ipfs/YourMetadataURI");

# Flujo de trabajo completo

RA (Unidad) :

El usuario coloca y personaliza una obra de arte en su entorno físico.
Los cambios se reflejan dinámicamente en la interfaz de RA.

Guardar Personalización (Firebase) :

La configuración se guarda en Firebase para persistencia y futuras referencias.

Acuñación de NFT (web3.js + Ethereum) :

La obra personalizada se convierte en un NFT único, almacenado en una blockchain pública como Ethereum.
Los metadatos (colores, marcos, estilos) se almacenan en IPFS o en la base de datos asociados.

Monedero e Interacción :

El NFT se transfiere al wallet del usuario, donde puede ser visualizado, vendido o compartido
