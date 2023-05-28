# zks2.01
// 获取地址在不同链的tx数量
function getTxCount(address,network) {
  
  // 根据 network 获取 RPC
  let rpcLink = RPC_MAP[network];
  if (!rpcLink) {
    return "Error: Invalid Network Name";
  }

  // 查询地址的交易数量
  const transactionRequest = {
    jsonrpc: "2.0",
    method: "eth_getTransactionCount",
    params: [address, "latest"],
    id: 1
  };
  const transactionResponse = UrlFetchApp.fetch(rpcLink, {
    method: "POST",
    payload: JSON.stringify(transactionRequest),
    headers: {
      "Content-Type": "application/json"
    }
  });
  const transactionCountHex = JSON.parse(transactionResponse.getContentText()).result;

  // 将十六进制转换为十进制
  const transactionCount = parseInt(transactionCountHex, 16);
