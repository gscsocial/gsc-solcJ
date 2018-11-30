# solcJ
Solidity compiler binaries, packed into jar for use in Java based projects.

We use jar in GSC project, And then we use code snippet for compilation:

```
String originAddress = "262daebb11f20b68a2035519a8553b597bb7dbbfa4";

String host = "127.0.0.1";
int port = 50051;
WalletGrpcClient walletBlockingStub = new WalletGrpcClient(host, port);

String contractSrc =
        "pragma solidity ^0.4.25;\n" +
                "contract cont {" +
                "        function name() returns (string) {return \"cont\";}\n" +
                "}";
SolidityCompiler.Result res = SolidityCompiler.compile(
        contractSrc.getBytes(), true, ABI, BIN, INTERFACE, METADATA);
System.out.println("Out: '" + res.output + "'");
System.out.println("Err: '" + res.errors + "'");

System.out.println("---------------------------------------------------------------------------------");

CompilationResult result = null;
try {
    result = CompilationResult.parse(res.output);
} catch (IOException e) {
    e.printStackTrace();
}
System.out.println("result: " + result.getContractName());

CompilationResult.ContractMetadata contractMetadata = result.getContract(result.getContractName());

String contractName = result.getContractName();
String abiStr = contractMetadata.abi;
String byteCode = contractMetadata.bin;
long callValue = 0L;
long consumeUserResourcePercent = 0;

Protocol.Transaction response = walletBlockingStub.deployContract(contractName, originAddress,
        abiStr, byteCode, callValue, consumeUserResourcePercent);

if (response != null){
    System.out.println("Contract: \n" + RPCUtils.printTransaction(response));
    System.out.println(response);
}
```
