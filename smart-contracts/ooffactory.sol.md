# OOFFactory.sol

This contract is used to generate new Open Oracle Framework Contracts.

### `oofMint`

```
function oofMint(
    address[] memory signers_,
    uint256 signerThreshold_,
    address payable payoutAddress_,
    uint256 subscriptionPassPrice_
)
external
returns(address oof)
```

�This function generates a new OOF contract with the values provided by cloning an existing Implementation of OOF to save gas and then calling the initialize function with the values provided as the arguments.

The contract also has the functions to change the ownership, update the Implementation address and the router address.

```
    /**
     * @dev gets the address of the current factory owner
     *
     * @return the address of the conjure router
    */
    function getConjureRouter() external view returns (address payable) {
        return conjureRouter;
    }

    /**
     * @dev lets the owner change the current conjure implementation
     *
     * @param oofImplementation_ the address of the new implementation
    */
    function newOOFImplementation(address oofImplementation_) external {
        require(msg.sender == factoryOwner, "Only factory owner");
        require(oofImplementation_ != address(0), "No zero address for oofImplementation_");

        oofImplementation = oofImplementation_;
        emit NewOOFImplementation(oofImplementation);
    }

    /**
     * @dev lets the owner change the current conjure router
     *
     * @param conjureRouter_ the address of the new router
    */
    function newConjureRouter(address payable conjureRouter_) external {
        require(msg.sender == factoryOwner, "Only factory owner");
        require(conjureRouter_ != address(0), "No zero address for conjureRouter_");

        conjureRouter = conjureRouter_;
        emit NewConjureRouter(conjureRouter);
    }

    /**
     * @dev lets the owner change the ownership to another address
     *
     * @param newOwner the address of the new owner
    */
    function newFactoryOwner(address payable newOwner) external {
        require(msg.sender == factoryOwner, "Only factory owner");
        require(newOwner != address(0), "No zero address for newOwner");

        factoryOwner = newOwner;
        emit FactoryOwnerChanged(factoryOwner);
    }

    /**
     * receive function to receive funds
    */
    receive() external payable {}
}

interface IOOF {
    function initialize(
        address[] memory signers_,
        uint256 signerThreshold_,
        address payable payoutAddress_,
        uint256 subscriptionPassPrice_,
        address factoryContract_
    ) external;
}
```

�
