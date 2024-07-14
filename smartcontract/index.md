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

## demo-link
 https://github.com/Ellioben/web3toblockchain
   

## 编写合约

准备工作

- 学习solidity语法

- 学习常见的合约ERC20，REC721，REC1155

  >Ethereum Request for Comment (ERC)，定义了符合规范的smart contract	
  >

- 学习测试网/模拟测试网工具使用

 后续有时间会补充solidity相关的...


# 开发名词解释

- Decentralized app
- decentralized finance
- Ethereum Request for Comment (ERC)定义了符合规范的smart contract	
- Smart contract（可以理解成交易脚本）



# Solidity

- Solidity是一门面向合约的、为实现智能合约而创建的高级编程语言。这门语言受到了C++， Python 和Javascript语言的影响，设计的目的是能在以太坊虚拟机(EVM)上运行。

- Solidity是静态类型语言，支持继承、库和复杂的用户定义类型等特性。

- 内含的类型除了常见编程语言中的标准类型，还包括 address等以太坊独有的类型， Solidity源码文件通常以。sol 作为扩展名

- 目前尝试Solidity编程的最好的方式是使用Remix。 Remix 是一个基于Web浏览器的IDE，它可以让你编写Solidity 智能合约，然后部署并运行该智能合约。

## 语法
### constructor /demo
```solidity
pragma solidity ^0.4.0;

contract StateVariables {
    //两个变数/属性
    string name;
    address owner;

    constructor() public{
        name = "unknow";
        owner = msg.sender;//初始化，执行者赋值给owner
    }

    function setName(string _name) public returns (string){
        if(msg.sender == owner){
            name = _name;
        } else {
            revert("Permission denied.");
        }
        return name;
    }
    
    function getName() public view returns(string){
        return name;
    }
}
```
### modifier
执行完这个modifier之后，会继续执行原本function该做的事。

```sol
pragma solidity ^0.4.0;

contract StateVariables {
    //两个变数/属性
    string name;
    address owner;

    constructor() public{
        name = "unknow";
        owner = msg.sender;//初始化，执行者赋值给owner
    }

    modifier onlyOwner(){
        require(msg.sender==owner,"Permission denied.");
        _;
    }

    function setName(string _name) public onlyOwner returns (string){
        name = _name;
        return name;
    }
    
    function getName() public view returns(string){
        return name;
    }
}
```

### ssert

```solidity
assert(condition);
```

- assert 表示断言

### Require
 ```
 require(condition, <String>);
 ```
 earlycheck

 ### Event & Log
Logs 包含  ：
- address:由m個 contract address所産生 
- blockHash, blockNumber, transactionHash, transactionIndex
- LogIndex：第幾個 log
- data: raw data (32 bytes 為單位) 

```
pragma solidity ^0.4.0;

contract test {
    //两个变数/属性
    string information;
    uint balance;
    event.LogCreate(string information,unit balance);
    event.LogCreate(string index information,unit balance);
    constructor() public{
        information = "xxxx";
        balance = 180;
        emit LogCreate (information ,balance)
        emit LogCreateIndex(information,balance)
    }
}
```
### Map
```
pragma solidity ^0.4.0;

contract test {

    mapping(address=>uint) public ledger;
    mapping(address=>bool) public donors;
    address[] public donorList;

    function isDonors(address Addr) internal view returns(bool){
        return donors[Addr];
    }
    function donate() public payable{
        if(msg.value>=1 ether){
            if(!isDonor(msg.sender)){
                donors[msg.sender] = true;
                donorList.push(msg.sender);
            }
            ledger[msg.sender]+=msg.value;
        }else{
            revert("< 1 Ether")
        }
    }
}
```

Struct

```
struct Student{
	string name；
	uint score;
	bool active;
}
```
Function 
```
pragma solidity ^0.4.25;

library Set {
    struct Data{
        mapping(int => bool) data;  
    }

    function Insert(Data strong self,int key) public returns (bool){
        if(self.data[key])
            return false;
        self.data[key] = true;
        return true;
    }

    function Remove(Data strong self,int key) public returns (bool) {
        if(!self.data[key])
            returns false;
        self.data[key] = false;
        return false;
    }

    function Contain(Data strong self,int key) public view return (bool){
        returns self.data[key];
    }
}


contract Mian{
    Set.Data set;
    function insert(int key )public returns (bool) {
        return Set.Insert(set,key);
    }

     function remove(int key) public returns (bool) {
        return Set.Remove(set,key);
    }

     function remove(int key) public view returns (bool) {
        return Set.Contain(set,key);
    }
}
```
