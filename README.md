本合约在ETH主网地址0xE84577B65ad14851eae08851E4A42fA31f78c5f4
    严重风险警告：仅供学习solidity用途，本合约可能非常不安全、暗藏巨大风险、可能导致使用人地址中ERC20代币在没有使用人签名情况下完全丢失、直接向本合约内部转入ERC20代币会永久导致其丢失在本合约中，没有任何方法可以找回
说明：本合约未经任何审计、完全开源、作者对solidity和ETH完全理解不清、作者完全不承担任何由之引起损失、本合约可能非常不安全、暗藏巨大风险、可能导致使用人地址中ERC20代币在没有使用人签名情况下完全丢失、直接向本合约内部转入ERC20代币会永久导致其丢失在本合约中，没有任何方法可以找回，没有UI可能导致输入错误、仅供学习用途，任何人不应该在主网中使用。
合约地址0xE84577B65ad14851eae08851E4A42fA31f78c5f4（ETH主网）
编译方法：solidity0.6.12,优化开启、200次
合约构造成本：352,522gas
参数说明：
function batchTransferEth(address payable[] memory to,uint256[] memory values) payable public	批量转账ETH给地址
参数：
to:地址数组，传入要转账给谁的地址
values:uint256数组，传入每个地址要转多少，这个数组长度和to数组要相同，如果这个地址更长，则浪费GAS，如果更短，则交易失败回滚
隐含参数value:转给合约的ETH数量，这个数量最好等于values数组和，否则合约会退还剩余ETH，导致消耗无谓的GAS
GAS消耗预计：
转账给10个新地址 350000左右
转账给10个有ETH地址 110000左右
function batchTransferErc20(address erc20Address,address payable[] memory to,uint256[] memory values) public	批量转账ERC20代币给地址，请注意，使用前需要去ERC20代币合约approve本合约，本交易会转账ERC20代币给合约，合约再转账给小号，转移数量为values数组求和。
erc20Address：ERC20代币地址
to:地址数组，传入要转账给谁的地址
values:uint256数组，传入每个地址要转多少，这个数组长度和to数组要相同，如果这个地址更长,浪费GAS，如果更短，则交易失败回滚
GAS消耗预计：
转账给10个新地址 350000左右
转账给10个有ERC20代币地址 180000左右
部署在ETH0xE84577B65ad14851eae08851E4A42fA31f78c5f4源代码
pragma solidity ^0.6.6;
interface IERC20 {
    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);

    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);

    function totalSupply() external view returns (uint);

    function balanceOf(address owner) external view returns (uint);

    function allowance(address owner, address spender) external view returns (uint);

    function approve(address spender, uint value) external returns (bool);

    function transfer(address to, uint value) external returns (bool);

    function transferFrom(address from, address to, uint value) external returns (bool);
}
contract BatchTransfer{
    function batchTransferEth(address payable[] memory to,uint256[] memory values) payable public{//添加payable,支持在调用方法的时候，value往合约里面传eth，注意该value最终平分发给所有账户
        //require(to.length == values.length);
        uint256 remainValue = msg.value;
        for(uint32 i=0;i<to.length;i++){
            require(remainValue >= values[i]);
            remainValue -= values[i];
            to[i].transfer(values[i]);
        }
        if(remainValue>0){
            msg.sender.transfer(remainValue);
        }
    }
    // function batchTransferErc20V0(address erc20Address,address payable[] memory to,uint256[] memory values) public{
    //这个方法不好，因为transferFrom操作比较消耗GAS,所以应该先把token划转到合约，再转账出去，批量转账3次内本方法较高，3次以上先划转的方法效率高
    //     //require(to.length == values.length);
    //     IERC20 erc20Token = IERC20(erc20Address);
    //     for(uint32 i=0;i<to.length;i++){
    //         erc20Token.transferFrom(msg.sender,to[i],values[i]);
    //     }
    // }
    function batchTransferErc20(address erc20Address,address payable[] memory to,uint256[] memory values) public{
        //require(to.length == values.length);
        uint256 totalValue = 0;
        for(uint32 i=0;i<to.length;i++){
            totalValue += values[i];
        }
        
        IERC20 erc20Token = IERC20(erc20Address);
        erc20Token.transferFrom(msg.sender,address(this),totalValue);
        for(uint32 i=0;i<to.length;i++){
            erc20Token.transfer(to[i],values[i]);
        }
    }
}

