本合约在ETH主网地址0xE84577B65ad14851eae08851E4A42fA31f78c5f4  
# 严重风险警告：仅供学习solidity用途，本合约可能非常不安全、暗藏巨大风险、可能导致使用人地址中ERC20代币在没有使用人签名情况下完全丢失、直接向本合约内部转入ERC20代币会永久导致其丢失在本合约中，没有任何方法可以找回  
# 说明：本合约未经任何审计、完全开源、作者对solidity和ETH完全理解不清、作者完全不承担任何由之引起损失、本合约可能非常不安全、暗藏巨大风险、可能导致使用人地址中ERC20代币在没有使用人签名情况下完全丢失、直接向本合约内部转入ERC20代币会永久导致其丢失在本合约中，没有任何方法可以找回，没有UI可能导致输入错误、仅供学习用途，任何人不应该在主网中使用。  
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
