# SmartContract


SmartContract

中文名称智能合约

> 了解智能合约签，现需要了解以太坊。

## 以太坊中重要概念

**账户(Account)**

包含地址，余额和随机数，以及可选的存储和代码的对象。

- 普通账户(EOA) ，存储和代码均为空

- 合约账户(Contract)，<u><font color='#ff441a' style="font-weight:bold">包含存储和代码</font></u>

**地址(Address)**

般来说，这代表一个EOA或合约，它可以在区块链上接收或发送交易。更具体地说，它是<u>ECDSA公钥的keccak散列的最右边的160位。</u>

**交易(Transaction)**

- 可以发送以太币和信息

- 向合约发送的交易可以调用合约代码，并以信息数据为函数参数
- 向空用户发送信息，可以自动生成以信息为代码块的合约账户 

**gas**

以太坊用于执行智能合约的虚拟燃料。以太坊虚拟机使用核算机制来衡量 gas的消耗量并限制计算资源的消耗。



## 以太坊特点

以太坊是“世界计算机”，这代表它是一个开源的、全球分布的计算基础设施

执行称为智能合约(smart contract)的程序

使用区块链来同步和存储系统状态以及名为以太币(ether)的加密货币，以计量和约束执行资源成本

<u><font color='#ff441a' style="font-weight:bold">本质是一个基于交易的状态机(transaction-based state machine)</font></u>

以太坊平台使开发人员能够构建具有内置经济功能的强大去中心化应用程序(**DApp**)；在持续自我正常运行的同时，它还减少或消除了审查，第三方界面和交易对手风险



## 名词解释

- ERC: Ethereum Request for Comments的缩写,以太坊征求意见.些EIP被标记为ERC，表示试图定义以太坊使用的特定标准的提议

- EOA： External Owned Accoupt，外部账户。由以太坊网络的人类用户创建的账户

- Keccak256：以太坊中使用的密码哈希函数。Keccak256被标准化为 SHA-3

- Nonce：在密码学中，术语nonce用于指代只能使用一次的值。以太坊使用两种类型的随机数，账户随机数和POW随机数





## Smart Contract

etherscan.io可以看到现在主网络上的合约交易记录。

### Remix

合约开发工具Remix（基于浏览器的Solidity在线编辑器）

> 合约都是solidity编写的，有必要学习一下solidity语法。

- VS code 下载Remix插件

- http://remix.ethereum.org/ 在线

- docker启动

  ```bash
  docker run -p 10330:80 remixproject/remix-ide:latest
  ```






### 其他合约开发工具

- MetaMask - 浏览器插件钱包

- Geth - 以太坊客户端(go语言)

- web3.js - 以太坊 javascipt API库

- Ganache-以太坊客户端(测试环境私链)

- Truffle- 以太坊开发框架
- Tenderly - Comprehensive Ethereum developer platform for real-time monitoring, alerting, debugging, and simulating smart contracts.
- Openzeppelin - 好用的Library  https://docs.openzeppelin.com/contracts/4.x/wizard



### 合约部署测试

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.7.0 <0.9.0;
/**
 * @title Storage
 * @dev Store & retrieve value in a variable
 */
contract Storage {
    uint256 number;
    function store(uint256 num) public {
        number = num;
    }
    function retrieve() public view returns (uint256){
        return number;
    }
}

```

DEPLOY&RUN

![image-20221008182827870](/img/image-20221008182827870.png)

<u><font color='#ff441a' style="font-weight:bold">可以通过测试方法进行输入参数和合约进行交互</font></u>





### 查看合约功能

> 通过查看ABI来查看合约提供的功能。

输出ABI

```json
[
	{
		"inputs": [
			{
				"internalType": "uint256",
				"name": "num",
				"type": "uint256"
			}
		],
		"name": "store",
		"outputs": [],
		"stateMutability": "nonpayable",
		"type": "function"
	},
	{
		"inputs": [],
		"name": "retrieve",
		"outputs": [
			{
				"internalType": "uint256",
				"name": "",
				"type": "uint256"
			}
		],
		"stateMutability": "view",
		"type": "function"
	}
]
```

### Web3

前端创建web3交互搭建

> 准备：安装google插件 MateMask

```js
   // <script src="web3.min.js"></script>

  <div>
    <button id="eth_requestAccounts">链接钱包</button>
  </div>

    jQuery("#eth_requestAccounts").click(async function () {
      if (typeof window.ethereum !== 'undefined') {
          let address=await ethereum.request({ method: 'eth_requestAccounts' });//授权连接钱包
          console.log('用户钱包地址:',address[0]);//第一个是meatmask的所有钱包
      }else{
          console.log('未安装钱包插件！');
      }
      //     ADDR = await ethereum.request({ method: 'eth_requestAccounts' });
      //     alert("连接Mask成功",ADDR)
    });

```

实例化web3

```js
 // 设置 web3 实例
    const web3 = new Web3("ws://localhost:7545")
    //合约地址
  	const Token = '0x06CE5a080b2a897dF3488150D2aD5440c334a423'


    // 获取所有的账户，用作后面发起交易时的账户
    const accounts = await web3.eth.personal.getAccounts();
    console.log("Test Wallet: ",accounts)

    // 载入合约
    const contract = new web3.eth.Contract(ABI, Token);

    // 调用ABI
    contract.methods.store(666)
    contract.methods.retrieve()
```



## 编写合约

准备工作

- 学习solidity语法

- 学习常见的合约ERC20，REC721，REC1155

  >Ethereum Request for Comment (ERC)，定义了符合规范的smart contract	
  >

- 学习测试网/模拟测试网工具使用

 后续有时间会补充solidity相关的...

