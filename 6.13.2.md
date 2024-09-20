# Ethers.js 6.13.2

## Install and Import
### Install
```
npm install ethers
```

### Import
```
import { ethers } from "ethers";
```

## Connect
### Metamask
```
let signer = null;

let provider;
if (window.ethereum == null) {
    console.log("MetaMask not installed; using read-only defaults")
    provider = ethers.getDefaultProvider()
} else {
    // Read access
    provider = new ethers.BrowserProvider(window.ethereum)
    
    // Write access
    signer = await provider.getSigner();
}
```
### RPC Provider
```
// Read access
provider = new ethers.JsonRpcProvider(url)

// Write access
signer = await provider.getSigner()
```

## Interaction
### Value conversion
```
// string (ETH) to number (Wei)
eth = parseEther("1.0")
// 1000000000000000000n

// string to number in customized unit
feePerGas = parseUnits("4.5", "gwei")
// 4500000000n

// number (Wei) to string (ETH)
formatEther(eth)
// '1.0'

// number to string in customized unit
formatUnits(feePerGas, "gwei")
// '4.5'
```
### Query blockchain
```
await provider.getBlockNumber()
// 20567263

balance = await provider.getBalance("ethers.eth")
// 4085267032476673080n

formatEther(balance)
// '4.08526703247667308'

await provider.getTransactionCount("ethers.eth")
// 2
```
### Send transaction
```
tx = await signer.sendTransaction({
  to: "ethers.eth",
  value: parseEther("1.0")
});

receipt = await tx.wait();
```
### Read contact
```
abi = [
  "function decimals() view returns (uint8)",
  "function symbol() view returns (string)",
  "function balanceOf(address a) view returns (uint)"
]

contract = new Contract("dai.tokens.ethers.eth", abi, provider)

sym = await contract.symbol()
// 'DAI'

decimals = await contract.decimals()
// 18n

balance = await contract.balanceOf("ethers.eth")
// 4000000000000000000000n

formatUnits(balance, decimals)
// '4000.0'
```
### Write contract
```
abi = [
  "function transfer(address to, uint amount)"
]

contract = new Contract("dai.tokens.ethers.eth", abi, signer)

amount = parseUnits("1.0", 18);

tx = await contract.transfer("ethers.eth", amount)

await tx.wait()
```


