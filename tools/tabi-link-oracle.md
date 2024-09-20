# Tabi Link Oracle

## Introduction

> Tabi Link includes Oracle and VRF, where Oracle represents an oracle that enables smart contracts to retrieve data from the outside world. VRF, also known as a verifiable random function, is a provably fair and verifiable random number generator (RNG) that enables smart contracts to access random values without affecting security or availability.



## I. Oracle

Working principle:

<figure><img src="https://rsrwqlwga4a.jp.larksuite.com/space/api/box/stream/download/asynccode/?code=YmFhZmUxNDM4ZTBlZjQwN2VlN2EyNjdkNjQyNjlmNTZfSEFPYUo5czk1MWRNQ2lQd01KTDZiMmpyd3NXU012ZXlfVG9rZW46UkxqOGJWU1Brb1pITWx4OTdmOGpGMHM4cGVlXzE3MTUwNzUxMDY6MTcxNTA3ODcwNl9WNA" alt=""><figcaption></figcaption></figure>

Consumer Address: 0xdD325193a3195b4654b12EaCFE0fE548C7761590&#x20;

Oracle address: 0x590311669252DCF34bfbF3981747D13Cf09ec19A&#x20;

Existing jobs are as follows:&#x20;

Get > Uint256 - (TOML) : 5b507ee5e7af477ebf31d4efaa5ba85b&#x20;

Get > Int256 - (TOML) : bcb8bc009e6042dd96727b61f4bd0238&#x20;

Get > Bool - (TOML) : ecacc544321640c399ddec5e99d6197f&#x20;

Get > String : a109c25f143d4adf9a5258628ad88bb2&#x20;

Get > Bytes : 264423c8ec534be6af0bb24ec8b1fdfa&#x20;

multi-word (TOML) : c7116b94377d41cb94b4fe9ea38e1d3a



Consumer contracts are as follows:

```TypeScript
// SPDX-License-Identifier: MIT
pragma solidity ^0.7.6;


import {Chainlink, ChainlinkClient} from "./ChainlinkClient.sol";
import {ConfirmedOwner} from "../shared/access/ConfirmedOwner.sol";

/**
 * THIS IS AN EXAMPLE CONTRACT THAT USES UN-AUDITED CODE.
 * DO NOT USE THIS CODE IN PRODUCTION.
 */

contract ATestnetConsumer is ChainlinkClient, ConfirmedOwner {
    using Chainlink for Chainlink.Request;

    uint256 public currentPrice;

    event RequestEthereumPriceFulfilled(
        bytes32 indexed requestId,
        uint256 indexed price
    );

    constructor() ConfirmedOwner(msg.sender) { }

    function requestEthereumPrice(
        address _oracle,
        string memory _jobId
    ) public onlyOwner {
        Chainlink.Request memory req = buildChainlinkRequest(
            stringToBytes32(_jobId),
            address(this),
            this.fulfillEthereumPrice.selector
        );
        req.add(
            "get",
            "https://min-api.cryptocompare.com/data/price?fsym=ETH&tsyms=USD"
        );
        req.add("path", "USD");
        req.addInt("times", 100);
        sendChainlinkRequestTo(_oracle, req, 0);
    }

    function fulfillEthereumPrice(
        bytes32 _requestId,
        uint256 _price
    ) public recordChainlinkFulfillment(_requestId) {
        emit RequestEthereumPriceFulfilled(_requestId, _price);
        currentPrice = _price;
    }

    function cancelRequest(
        bytes32 _requestId,
        uint256 _payment,
        bytes4 _callbackFunctionId,
        uint256 _expiration
    ) public onlyOwner {
        cancelChainlinkRequest(
            _requestId,
            _payment,
            _callbackFunctionId,
            _expiration
        );
    }

    function stringToBytes32(
        string memory source
    ) private pure returns (bytes32 result) {
        bytes memory tempEmptyStringTest = bytes(source);
        if (tempEmptyStringTest.length == 0) {
            return 0x0;
        }

        assembly {
        // solhint-disable-line no-inline-assembly
            result := mload(add(source, 32))
        }
    }
}
```

\
Request code:

```TypeScript
import { ethers } from "hardhat";

async function main() {
  const ATestnetConsumer = await ethers.deployContract("ATestnetConsumer");

  await ATestnetConsumer.waitForDeployment();

  console.log("ATestnetConsumer:", ATestnetConsumer.target);

  const Operator = "0x590311669252DCF34bfbF3981747D13Cf09ec19A"
  const job = "5b507ee5e7af477ebf31d4efaa5ba85b"

  const result = await (await ATestnetConsumer.requestEthereumPrice(Operator, job)).wait()

  // @ts-ignore
  const ChainlinkRequested = ATestnetConsumer.interface.parseLog(result.logs[0])

  console.log("ChainlinkRequested:", ChainlinkRequested, result?.hash)


  // @ts-ignore
  await ATestnetConsumer.once("RequestEthereumPriceFulfilled", (_requestId, _price) => {
    console.log("_requestId:", _requestId, " price:", _price)
  })

}

// We recommend this pattern to be able to use async/await everywhere
// and properly handle errors.
main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
```

The results are as follows:

```Plain
npx hardhat run scripts/deploy_ATestnetConsumer.ts  --network tabi
ATestnetConsumer: 0xdD325193a3195b4654b12EaCFE0fE548C7761590
request tx hash: 0x8b3270449f3690e0592458ff836afdbed9955cdcbda20ac7c82535b66636c3bf
_requestId: 0xa95ad018d2d2eb8f728b2cabe8bdcc38c2a89dba422c88b5af0aaa868a6d74ff  price: 342234n
```

Detailed code can be found in [tabi-oracle](https://github.com/tabilabs/tabi-oracle)\


## II. VRF

The specific process is shown in the figure.

<figure><img src="https://rsrwqlwga4a.jp.larksuite.com/space/api/box/stream/download/asynccode/?code=NTgxNjJiMjkxNTczNDg4MWVkNDczN2Q4MGY5OGI2ZWFfbENoSGVMaVhleXJtMHRzRlVpMkVIaTBjRVVZU1FEWU1fVG9rZW46TFpnT2JwTzFUb0xNcWF4Vm9NRGpuREM1cFFUXzE3MTUwNzUxMDY6MTcxNTA3ODcwNl9WNA" alt=""><figcaption></figcaption></figure>

VRF Consumer Address: 0xb484B5F803F912C074Ac204dC66114F06aBc2100

VRF Coordinator Address: 0x9492b270EdA7d4046D6aa5e3F15c24deD2c8BD25

The contract code for **VRFConsumer.sol** is as follows:

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

 import "./VRFConsumerBase.sol";
 import "./VRFCoordinator.sol";
 
 import "@openzeppelin/contracts-upgradeable/access/OwnableUpgradeable.sol";

contract VRFConsumer is VRFConsumerBase, OwnableUpgradeable
{
    event RequestSent(uint256 requestId, uint32 numWords);
    event RequestFulfilled(
        uint256 requestId,
        uint256[] randomWords
    );
    error InsufficientFunds(uint256 balance, uint256 paid);
    error RequestNotFound(uint256 requestId);

    struct RequestStatus {
        bool fulfilled; // whether the request has been successfully fulfilled
        uint256[] randomWords;
    }
    mapping(uint256 => RequestStatus)
        public s_requests; /* requestId --> requestStatus */

    // past requests Id.
    uint256[] public requestIds;
    uint256 public lastRequestId;

    VRFCoordinator public VrfCoordinator;

    constructor(
        address vrfCoordinator
    ) VRFConsumerBase(vrfCoordinator)
    {
        VrfCoordinator = VRFCoordinator(vrfCoordinator);
        initialize();
    }

    function initialize() public initializer {
        __Ownable_init();
    }

    function requestRandomWords(
        uint32 _callbackGasLimit,
        uint32 _numWords
    ) external onlyOwner returns (uint256 requestId) {
        (uint16 requestConfirmations, , bytes32[] memory keyHash) = VrfCoordinator.getRequestConfig();
        requestId = VrfCoordinator.requestRandomWords(keyHash[0], requestConfirmations, _callbackGasLimit, _numWords);

        s_requests[requestId] = RequestStatus({
            randomWords: new uint256[](0),
            fulfilled: false
        });
        requestIds.push(requestId);
        lastRequestId = requestId;
        emit RequestSent(requestId, _numWords);
        return requestId;
    }

    function fulfillRandomWords(
        uint256 _requestId,
        uint256[] memory _randomWords
    ) override internal {
        RequestStatus storage request = s_requests[_requestId];

        require(!request.fulfilled, "Fulfilled");
        request.fulfilled = true;
        request.randomWords = _randomWords;

        emit RequestFulfilled(_requestId, _randomWords);
    }

    function getNumberOfRequests() external view returns (uint256) {
        return requestIds.length;
    }

    function getRequestStatus(
        uint256 _requestId
    )
        external
        view
        returns (bool fulfilled, uint256[] memory randomWords)
    {
        RequestStatus memory request = s_requests[_requestId];
        return (request.fulfilled, request.randomWords);
    }

}
```

VRFConsumerBase.sol contract:

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

abstract contract VRFConsumerBase {
  error OnlyCoordinatorCanFulfill(address have, address want);
  address private immutable vrfCoordinator;

  /**
   * @param _vrfCoordinator address of VRFCoordinator contract
   */
  constructor(address _vrfCoordinator) {
    vrfCoordinator = _vrfCoordinator;
  }

  /**
   * @notice fulfillRandomness handles the VRF response. Your contract must
   * @notice implement it. See "SECURITY CONSIDERATIONS" above for important
   * @notice principles to keep in mind when implementing your fulfillRandomness
   * @notice method.
   *
   * @dev VRFConsumerBase expects its subcontracts to have a method with this
   * @dev signature, and will call it once it has verified the proof
   * @dev associated with the randomness. (It is triggered via a call to
   * @dev rawFulfillRandomness, below.)
   *
   * @param requestId The Id initially returned by requestRandomness
   * @param randomWords the VRF output expanded to the requested number of words
   */
  function fulfillRandomWords(uint256 requestId, uint256[] memory randomWords) internal virtual;

  // rawFulfillRandomness is called by VRFCoordinator when it receives a valid VRF
  // proof. rawFulfillRandomness then calls fulfillRandomness, after validating
  // the origin of the call
  function rawFulfillRandomWords(uint256 requestId, uint256[] memory randomWords) external {
    if (msg.sender != vrfCoordinator) {
      revert OnlyCoordinatorCanFulfill(msg.sender, vrfCoordinator);
    }
    fulfillRandomWords(requestId, randomWords);
  }
}
```

\
\
The request code is as follows:

```JavaScript
import {ethers} from "ethers";
import abi_VRFConsumer from "./abi_VRFConsumer.json" assert { type: "json" }
import abi_VRFCoordinator from "./abi_VRFCoordinator.json" assert { type: "json" }

const main = async () => {
    const tabiRpc = "https://rpc.testnet.tabichain.com/"
    const vrfConsumerAddr = "0xb484B5F803F912C074Ac204dC66114F06aBc2100"
    const privateKey = 'YOUR_PRIVATE_KEY'

    const provider = new ethers.JsonRpcProvider(tabiRpc);

    const wallet = new ethers.Wallet(privateKey, provider)

    const vrfConsumer = ethers.Contract.from(vrfConsumerAddr, abi_VRFConsumer, wallet)

    const requestId = await requestRandomWords(vrfConsumer)

    console.log("requestRandomWords requestId:", requestId)

    const randomWords = await listener(vrfConsumer, requestId)

    console.log("randomWords:", randomWords)
}

const requestRandomWords = async (vrfConsumer) => {
    const callbackGasLimit = 400000
    const numWords = 4
    const result = await (await vrfConsumer.requestRandomWords(callbackGasLimit, numWords)).wait()

    console.log("requestRandomWords hash:", result.hash)

    const RandomWordsRequested = ethers.Interface.from(abi_VRFCoordinator).parseLog(result.logs[0])

    return RandomWordsRequested.args.requestId
}


const listener = async (vrfConsumer, requestId) => {
    return new Promise((resolve) => {
         vrfConsumer.once("RequestFulfilled", (_requestId, _randomWords) => {
            if (requestId === _requestId) {
                resolve(_randomWords)
            }
        })
    })
}


main()
```

_See_ [_VRF Example_](https://github.com/tabilabs/vrf-example)\
\
\
\
\
