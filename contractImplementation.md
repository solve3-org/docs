## Introduction <!-- {docsify-ignore} -->


The `Solve3Verify.sol` contract is a critical component of the [Solve3](https://solve3.org) bot protection system. It enables you to verify Solve3 proofs and ensure secure interactions within your smart contracts. This documentation provides a detailed guide on integrating and using the `Solve3Verify` contract in your Ethereum-based projects.

## Quick Start <!-- {docsify-ignore} -->

Integrating the `Solve3Verify` contract into your project is a straightforward process. Here's a quick start guide:

### Import Solve3Verify <!-- {docsify-ignore} -->

Begin by importing the `Solve3Verify` contract into your Solidity project.

```solidity
import "@solve3/contracts/Solve3Verify.sol";
```

### Inherit from Solve3Verify <!-- {docsify-ignore} -->

In your contract, inherit from the `Solve3Verify` contract.

```solidity
contract YourContract is Solve3Verify {
    // Your contract code here
}
```

### Initialize the Contract <!-- {docsify-ignore} -->

In your contract's constructor, call the `__init_Solve3Verify` function with the address of the Solve3 Master contract as an argument.

```solidity
constructor(address _solve3Master) {
    __init_Solve3Verify(_solve3Master);
    // Your constructor code here
}
```

### Add solve3Verify modifier <!-- {docsify-ignore} -->

To verify the proof created by Solve3 you have to add the `solve3Verify` modifier to your function and pass the `_proof` parameter.

```solidity
function foo(bytes memory _proof) external solve3Verify(_proof) {
    // Your function implementation
}
```

## Abstract Function disableSolve3 <!-- {docsify-ignore} -->

Since Solve3 is in beta, you have the option to disable Solve3 verification in your contract. By using the `disableSolve3` function, you can control whether Solve3 verification is enabled or disabled. This is particularly useful for scenarios where you need to bypass Solve3 temporarily.

!> **Important**: When disabling Solve3 verification, you need to pass `0x` as the `proof` parameter when calling functions that have the `solve3Verify` modifier. The contract will execute the function without Solve3 verification if `0x` is provided as the proof.

Here's how to use the `disableSolve3` function:

```solidity
function disableSolve3(bool _flag) external override onlyOwner {
    _disableSolve3(_flag);
}
```

Your contract is now ready to verify Solve3 proofs for secure interactions.

## Advanced Options

The `Solve3Verify` contract provides optional functions to enhance flexibility and control in your project based on your personal needs or the chain you want to deploy to.

### Set Valid From Timestamp

To customize the timestamp from which the signature is valid, you can implement the following internal function:

```solidity
function _setValidFromTimestamp(uint256 _validFromTimestamp) internal {
    validFromTimestamp = _validFromTimestamp;
}
```

This function allows you to adjust the timestamp based on your specific requirements. 

**Note:** Per default the `validFromTimestamp` is set to timestamp of the contract deployment.

### Set Valid Period Seconds

You can modify the period in seconds for which the signature is valid using this internal function:

```solidity
function _setValidPeriodSeconds(uint256 _validPeriodSeconds) internal {
    validPeriodSeconds = _validPeriodSeconds;
}
```

Use this function to change the period for which the signature remains valid.

**Note:** Per default the `validPeriodSeconds` is set to 300 seconds.

Developers can customize the `validFrom` and `validPeriod` functionality by overriding the following functions:

### Custom validFrom Implementation

`function validFrom() public view virtual returns (uint256)`

This overridable function allows developers to modify the timestamp from which the signature is considered valid.

```solidity
function validFrom() public view virtual returns (uint256) {
    // Custom logic to determine the validFrom timestamp
    return yourCustomValidFromTimestamp;
}
```

### Custom validPeriod Implementation

 `function validPeriod() public view virtual returns (uint256)`
 
 This overridable function enables developers to change the period in seconds for which the signature remains valid.

```solidity
function validPeriod() public view virtual returns (uint256) {
    // Custom logic to determine the valid period in seconds
    return yourCustomValidPeriodSeconds;
}
```

These functions provide flexibility for developers to adapt Solve3 verification to their specific project requirements.


## Public Variables and View Functions

The `Solve3Verify` contract includes various public variables and view functions for inspecting its state. These include:

* `public solve3Master`: The address of the Solve3 Master contract.
* `public solve3Disabled`: A flag indicating whether Solve3 verification is disabled.
* `public validFromTimestamp`: The timestamp from which the signature is valid.
* `public validPeriodSeconds`: The period in seconds for which the signature is valid.