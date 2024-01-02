# Solidity Smart Contracts Library and Bep20 Smart Contract
 
Properties of functions in a contract written in Solidity

## 1. Proxy Contract (Proxy.sol):
This is an abstract contract providing the basic functionality for delegate calls and fallback methods. The key methods are:
* _delegate(address implementation): Performs a delegate call to the specified implementation address.
* _implementation(): Should be implemented by derived contracts to return the current implementation address.
* _fallback(): Initiates a delegate call to the current implementation address when no specific function matches the call data.

## 2. Beacon Interface (IBeacon.sol):
An interface defining a single method:
* implementation(): This method must be implemented by a beacon contract, returning the address of the current implementation contract.

## 3. Address Library (Address.sol):
A library providing utility functions for working with Ethereum addresses:
* isContract(address account): Checks if the given address corresponds to a contract.
* sendValue(address payable recipient, uint256 amount): Safely sends a specified amount of Ether to a recipient.
* functionCall(...), functionCallWithValue(...), etc.: Low-level functions for making various types of function calls to contracts.

## 4. Storage Slot Library (StorageSlot.sol):
A library facilitating reading and writing primitive types to specific storage slots. It's designed to avoid storage conflicts when dealing with upgradeable contracts.

## 5. ERC1967 Upgrade Library (ERC1967Upgrade.sol):
An abstract contract providing getters, event-emitting update functions, and storage slots for EIP-1967 upgradeable contracts. Notable elements include:
* _IMPLEMENTATION_SLOT: A constant defining the storage slot for the current implementation address.
* _getImplementation(), _setImplementation(address newImplementation): Functions for getting and setting the current implementation.
* _upgradeTo(address newImplementation): Upgrades the contract to a new implementation.
* _upgradeToAndCall(address newImplementation, bytes memory data, bool forceCall): Upgrades the contract and optionally calls a function on the new implementation.
* _ADMIN_SLOT: A constant defining the storage slot for the admin address.
* _getAdmin(), _setAdmin(address newAdmin): Functions for getting and setting the admin address.
* _changeAdmin(address newAdmin): Changes the admin address.

## 6. ERC1967 Proxy (ERC1967Proxy.sol):
A contract implementing an upgradeable proxy following the EIP-1967 standard. Key elements include:
* _implementation(): Overrides the abstract function from Proxy.sol to return the current implementation address.

## 7. Transparent Upgradeable Proxy (TransparentUpgradeableProxy.sol):
Extends ERC1967Proxy and adds functionality for managing the admin. The admin is the address that has the authority to perform certain actions, such as upgrading the implementation. Key elements include:
* ifAdmin modifier: A modifier ensuring that only the admin can execute certain functions.
* _admin(): Overrides the function from ERC1967Upgrade.sol to return the current admin address.
* _beforeFallback(): Ensures that the admin cannot access the fallback function, enhancing security.

## Overall Flow:
*       TransparentUpgradeableProxy is deployed with an initial implementation (_logic), admin address, and optional initialization data (_data).
*       The proxy can be used to interact with the functionality of the initial implementation.
*       The admin can upgrade the implementation to a new contract while preserving the proxy's storage state.
*       The admin can also change, and other functionalities can be performed using the proxy.

This pattern allows for upgrades to the contract logic while maintaining the state stored in the proxy. The proxy is transparent in the sense that it allows direct access to the state of the logic contract.

