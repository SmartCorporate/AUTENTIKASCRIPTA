Diario di Bordo:


===========================================================================

Codice solidity REMIX:

// SPDX-License-Identifier: MIT
//100000000000000000 WEY = 0.1ETH che poi saranno BNB
//AutentikaScriptaV1
//modificato per essere usato in Binance Smart Chain
//Developer: Michele Zaniolo Crypto 2022 UnitedStates

pragma solidity ^0.8.13;

contract AutentikaScripta {
    // State variable to store a number
    string public TestoUtente;
    uint public IDnumerica = 12500000; // Inizializza il contatore a 12500000 per far partire un numer da 8 cifre a crescere +1
    
    address feeAddress = 0x28796154A3B187724CDfcA37e0cdAFe60BD86b78;//Metamask Deposito Fee
   
    mapping(uint => string) public ArchiviTesti; //Archivio chiave vlore ID --> TestoUnico

function ScriviInBlockchain(string memory _Unico) public payable returns (uint) { 
    require(msg.value == 0.1 ether, "A Process fee of 0.1 BNB is required");
    address payable payableFeeAddress = payable(feeAddress); // Converti feeAddress in un oggetto di tipo address payable usando il costruttore payable
    payableFeeAddress.transfer(msg.value); // Invia la fee all'indirizzo specificato
    TestoUtente = _Unico;
    IDnumerica++; // Incrementa il contatore di 1
    ArchiviTesti[IDnumerica] = _Unico; // Aggiungi il testo e l'ID al mapping
    return IDnumerica; // Restituisci l'ID numerico
}

function LeggiDaID(uint _id) public view returns (string memory) {
    return ArchiviTesti[_id];
}

   
}



=======================================

HTML INTERFACCIA UI WEB su Visual Studio index.html




<html>
<head>
    <title> AutentikaScripta </title>
</head>
<body>
    <body style="background-color: orange;"></body>
    <style> button:hover { background-color: rgb(60, 255, 0);}
    button {
  display: inline-block;
  background-color: #7b38d8;
  padding: 10px;
  width: 290px;
  color: #ffffff;
  text-align: center;
  border: 4px double #cccccc; /* add this line */
  border-radius: 25px; /* add this line */
  font-size: 18px; /* add this line */
}
    </style>
      <hr>
  
    <button onclick="useHelp()">HELP come funziona la pagina</button>
    
   
  
    <button onclick="connectToMetaMask()" style="position: absolute; top: 10px; right: 150px;">Connetti a MetaMask</button>
    <p id="accountArea"    style="position: absolute; top: 40px; right: 150px;" > WALLET ADDRESS ? </p>
    <br>
    <br>
    <button onclick="connectContract()" style="position: absolute; top: 90px; right: 150px;">Check Connessione Web3</button>
    <p id="contractArea"    style="position: absolute; top: 120px; right: 150px;" > SMARTCONTRACT READY ? </p>
    <br>
    <br>
    <button onclick="readIdNumber()" style="position: absolute; top: 170px; right: 150px;">Check ultimo ID usato</button>
    <p id="IdArea"    style="position: absolute; top: 200px; right: 150px;" > NUMERO ID ? </p>
    
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <hr>

    <h1 style="position: absolute; top: 50px; ">Interfaccia Web3 SmartContract AutentikaScripta V1</h1>
   
    <!-- Includi la libreria web3 -->
    <script src="https://cdn.jsdelivr.net/npm/web3@latest/dist/web3.min.js"></script>

  
   
   
   <script>
        
        // Sostituisci il seguente indirizzo con quello dell'indirizzo che ha deployato il contratto
        var contractOwnerAddress = "0x28796154A3B187724CDfcA37e0cdAFe60BD86b78";
        


        // 1- Inizializza METAMASK web3
        let account;
        const connectToMetaMask = async () => {
            if (window.ethereum !== "non riconosciuto") {
                const accounts = await ethereum.request({ method: "eth_requestAccounts"});
                account = accounts[0];
                document.getElementById("accountArea").innerHTML = "WALLET ADDRESS=  " + account;
            }
        }    


        //2- connessione allo smartcontract
        const connectContract = async () => {
            const ABI = [
	{
		"inputs": [
			{
				"internalType": "string",
				"name": "_Unico",
				"type": "string"
			}
		],
		"name": "ScriviInBlockchain",
		"outputs": [
			{
				"internalType": "uint256",
				"name": "",
				"type": "uint256"
			}
		],
		"stateMutability": "payable",
		"type": "function"
	},
	{
		"inputs": [
			{
				"internalType": "uint256",
				"name": "",
				"type": "uint256"
			}
		],
		"name": "ArchiviTesti",
		"outputs": [
			{
				"internalType": "string",
				"name": "",
				"type": "string"
			}
		],
		"stateMutability": "view",
		"type": "function"
	},
	{
		"inputs": [],
		"name": "IDnumerica",
		"outputs": [
			{
				"internalType": "uint256",
				"name": "",
				"type": "uint256"
			}
		],
		"stateMutability": "view",
		"type": "function"
	},
	{
		"inputs": [
			{
				"internalType": "uint256",
				"name": "_id",
				"type": "uint256"
			}
		],
		"name": "LeggiDaID",
		"outputs": [
			{
				"internalType": "string",
				"name": "",
				"type": "string"
			}
		],
		"stateMutability": "view",
		"type": "function"
	},
	{
		"inputs": [],
		"name": "TestoUtente",
		"outputs": [
			{
				"internalType": "string",
				"name": "",
				"type": "string"
			}
		],
		"stateMutability": "view",
		"type": "function"
	}
];
            const Address = "0xf1c83982D7CFC30e7Ea6dD46B99fDE198D8634Ab";
            window.web3 = await new Web3(window.ethereum);
            window.contract = await new window.web3.eth.Contract( ABI, Address);
            document.getElementById("contractArea").innerHTML = "SMARTCONTRACT=  0xf1c83982D7CFC30e7Ea6dD46B99fDE198D8634Ab CONNESSO CORRETTAMENTE";
        }


        //3- leggi dato ID attuale dallo smart contract
        const readIdNumber = async () => {
        const data = await window.contract.methods.IDnumerica().call();
        document.getElementById("IdArea").innerHTML = data;  
        IDvariabile=document.getElementById('input-id').value=data;  
        }


        //4- Leggi stringa in base al ID inserita
        const leggiTesto = async (IDvariabile) => {
            IDvariabile=document.getElementById('input-id').value;
        const stringa = await window.contract.methods.LeggiDaID(IDvariabile).call();
        document.getElementById("stringaArea").innerHTML = stringa;    
        }


        //5- Scrivi testo in blockchain pagando 0.1BNB(100000000000000000 wey) con gas minimo di 1000000
        const scriviTesto = async () => {
            alert("Assicurati di avere almeno 0.1BNB e di essere connesso alla BSC Chain!");
            const TestoScritto = document.getElementById('text-area').value;
            await window.contract.methods.ScriviInBlockchain(TestoScritto).send({ 
                from: account,
                value: '100000000000000000',
                gas: '1000000'
             });
           

          
        }






async function getAccount() {
   const showAccount = document.querySelector('.showAccount');
   const accounts = await ethereum.request({ method: 
   'eth_requestAccounts' });
   const account = accounts[0];
   showAccount.innerHTML = account;
}

    </script>





    <!-- Crea un form per inserire il testo da scrivere nella blockchain -->
    <form>
        <label for="input-testo">-Inserisci il testo da scrivere permanentemente nella blockchain max 256 caratteri (Esempio: inserisci dati importanti e chiari come data,luogo, nome Articolo, caratteristiche prodotto, ecc)</label><br>
        <label>-Appena avrai scritto, controlla e se sei sicuro, premi il pulsate sotto "Scrivi testo nella Blockchain"  e quando Metamask avra' eseguito la transazione , ricordati di leggere quale e' NUMERO ID (check ultimoID usato)  e scrivitelo/salvatelo (operazione singola non ripetibile!)</label><br>
        <!-- <input type="text" id="input-testo" name="testo"><br> -->
        <textarea id="text-area" rows="6" cols="120" ></textarea>
        <br>
        <button type="button" onclick="scriviTesto()">Scrivi testo nella Blockchain</button>
    
    
    </form>

       <hr>

    <!-- Crea un form per inserire l'ID del testo da leggere -->
    <form>
        <label for="input-id">Inserisci NUMERO ID  (8 cifre) del testo da leggere:</label><br>
        <input type="number" id="input-id" name="id"><br>
        
        
        <button type="button" onclick="leggiTesto()">Leggi testo dalla Blockchain</button>
        <br>
    <br>
        <p id="stringaArea" style="font-weight: bold; font-size: larger; background-color: silver; position: absolute; top: 560px; left: 20px;">Contenuto Stringa=</p>
      </form>

    <br>
    <br>
    <br>
   
    
    <hr>
    <body>
       
       
<script>
function useHelp() {
  alert("Con questa web app, che utilizza uno smartcontract web3 sulla rete di Binance, è possibile scrivere in modo permanente sulla blockchain un testo di massimo 256 caratteri al costo di 0.1 BNB e associarlo ad un NUMERO ID univoco di 8 cifre. Per utilizzare questa funzionalità, è necessario che l'utente della pagina web 3 abbia installato e configurato un portafoglio Metamask con alcuni fondi in BNB. Tieni inoltre presente che la lettura dei testi tramite ID univoco è gratuita, mentre per la scrittura è necessario disporre di fondi BNB.  I passaggi devono essere cosi: 1)Premi Connetti A Metamask  2)Premi Check Connessione Web3  3)Scrivi il testo nel riquadro bianco e premi pulsante: Scrivi testo nella blockChain e attendi conferma di avvenuta transazione in Metamask  4)Premi Check ultimo ID Usato  5)Salvati NUMERO ID  6)Inserici NUMERO ID nella casella NUMERO IDdel testo da leggere  7)Premi Leggi testo dalla Blockchain");
}
</script>
<a href='https://www.bscscan.com/address/0xf1c83982d7cfc30e7ea6dd46b99fde198d8634ab/'>Link To SmartContract Binance Page</a>




    </body>
    <hr>
    


    

    



 

    


    
=======================================

ho dovuto creare un server locale per far funzionare il collegamento con metamask

ho installato sulla cartella finale del progetto :
npm install express --save


poi per farlo partire :
node server.js


poi ho creato un file server.js con dentro questo codice:

const express = require("express");
const path = require("path");

const app = express();

app.get("/", (req, res) => {
  res.sendFile(path.join(__dirname + "/index.html"));
});

const server = app.listen(5000);
const portNumber = server.address().port;
console.log("Server port number listen 127.0.0.1:" + portNumber);



========================================================

Prima scrittura sullo smartcontract per testare archiviazione

Testo usato:

Il 26 Dicembre 2022, io Michele Zaniolo ho avuto l'opportunità di scrivere il primo testo permanentemente utilizzando lo smartcontract AutentikaScriptaV1. 

Posso controllarlo qui:
https://www.bscscan.com/tx/0x5d7bdb9dc0a28de04619c1cf74872e9f285b0fcb3279feda0fce6b61d3da5767


Binance  Metamask su chrome address feeAddress = 0x28796154A3B187724CDfcA37e0cdAFe60BD86b78;

contratto smartcontract:  0xf1c83982D7CFC30e7Ea6dD46B99fDE198D8634Ab

