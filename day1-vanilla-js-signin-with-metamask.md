Context: 
- On a web 3 app, signing in with Ethereum and conencting wallet to Ethereum is a fundamental concept. In today's task, I'll be working on using the least amount of code to connect wallet to a web page .






Code 
```html

<html>

<head>

    <script src="https://cdn.ethers.io/lib/ethers-5.2.umd.min.js" type="application/javascript"></script>

    <script>
        // A Web3Provider wraps a standard Web3 provider, which is
        // what MetaMask injects as window.ethereum into each page
        const provider = new ethers.providers.Web3Provider(window.ethereum)

        // MetaMask requires requesting permission to connect users accounts
        provider.send("eth_requestAccounts", []);

        // The MetaMask plugin also allows signing transactions to
        // send ether and pay to change state within the blockchain.
        // For this, you need the account signer...
        const signer = provider.getSigner();

        (async function () {
            let userAddress = await signer.getAddress();
            document.getElementById("wallet").innerText =
                "Your wallet is " + userAddress;

            let blockNumber = await provider.getBlockNumber();

            document.getElementById("block").innerText = "current block number is: " + blockNumber;

        })();
    </script>

</head>

<body>
    Hello

    <p id="wallet"></p>

    <p id="block"></p>

</body>


</html>


```


# Frameworks and tools used: 
- Ethers.js - https://docs.ethers.io/v5/getting-started/

# References: 
- IIFE (Immediately Invoked Function Expression) : https://developer.mozilla.org/en-US/docs/Glossary/IIFE

