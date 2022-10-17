1. Write a contract which deploys a contract.

Method 1:
contract Child {
string public name;
string public gender;

    constructor(string memory _name, string memory _gender) payable {
        name = _name;
        gender = _gender;
    }

}

contract Parent{

    Child public childContract;

    function createChild(string memory _name, string memory _gender) public payable returns(Child) {

require(msg.value == 0.005 ether)
childContract = new Child{value: msg.value}(\_name, \_gender); // creating new contract inside another parent contract
return childContract;
}
}

Method 2:
We can use create2 to deploy a contract from a contract.

What is a fallback function ?

The solidity fallback function is executed if none of the other functions match the function identifier or no data was provided with the function call. Only one unnamed function can be assigned to a contract and it is executed whenever the contract receives plain Ether without any data. To receive Ether and add it to the total balance of the contract, the fallback function must be marked payable. If no such function exists, the contract cannot receive Ether through regular transactions and will throw an exception.

abi.encode vs abi.encodepacked ?

abi.encode encode its parameters using the ABI specs. The ABI was designed to make calls to contracts. Parameters are padded to 32 bytes. If you are making calls to a contract you likely have to use abi.encode

abi.encodePacked encode its parameters using the minimal space required by the type. Encoding an uint8 it will use 1 byte. It is used when you want to save some space, and not calling a contract.

How is overflow handled in 0.8 ?

Arithmetic operations in Solidity wrap on overflow. This can easily result in bugs, because programmers usually assume that an overflow raises an error, which is the standard behavior in high level programming languages. SafeMath restores this intuition by reverting the transaction when an operation overflows.

Explain ecrecover with an example.

v, r, s are the values for the transaction's signature. They can be used as in Get public key of any ethereum account

function recoverSigner(bytes memory sig,bytes32 \_hash) //signature, hash
internal
pure
returns (address)
{
(uint8 v, bytes32 r, bytes32 s) = splitSignature(sig);
return ecrecover(bytes32(\_hash), v, r, s);
}
function splitSignature(bytes memory sig)
internal
pure
returns (
uint8 v,
bytes32 r,
bytes32 s
)
{
require(sig.length == 65);
assembly {
// first 32 bytes, after the length prefix.
r := mload(add(sig, 32))
// second 32 bytes.
s := mload(add(sig, 64))
// final byte (first byte of the next 32 byt (es).
v := byte(0, mload(add(sig, 96)))
}
return (v, r, s);
}

6. Storage vs memory vs calldata.

Memory and calldata mean the variable will only exist for the transaction, storage will be saved forever. Memory is used for function parameters and function scoped variables, calldata is memory but immutable (and cheaper). Storage for state variables.

7. Create a Solidity contract with one function
   The function should return the amount of ETH that was passed to it, and the function
   body should be written in assembly.

contract hello {

    function getData() public payable returns(uint256) {
        assembly{
            return: msg.value;
        }

    }

}
