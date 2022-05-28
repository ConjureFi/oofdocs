# Time Weighted Average Price using OOF

You can use our custom TWAP tool with all OOF contracts that have historical tracking (most), the contract for TWAPS uses a simple call with an interface, and allows you to combine all TWAPsa for all feeds you want in a single call to save gas. You simply supply the timestamp for the start and the timestamp for the end and itll construct the TWAP for you for the feed.&#x20;

This allows high flexibility and lets you choose the TWAP for your need. The contract has a flag strictMode which if set will make it so that if the oracle doesn't have enough history for the range at the start and at the end to both have a value, the call will revert. So if a oracle only has 2w in prices and you try to constructac a 4w TWAP, itll return the 2w in values that it has available. This allows you to revert if the oracle doesnt have enough to be as called, while allowing contracts where you just want as much as possible to still succeed.   There are 2 functions

`function getTWAP(address OOFContract, uint256[] memory feedIDs, uint256[] memory timestampstart, uint256[] memory timestampfinish, bool strictMode) external view returns (uint256[] memory TWAP);`

`function lastTWAP(IOpenOracleFramework OOFContract, uint256[] memory feedIDs, uint256[] memory times, bool strictMode) external view returns (uint256[] memory TWAP)`

LastTWAP allows for you to simply enter a time in seconds back to use to construct a TWAP, say you want a 1h TWAP, you simply supply 360000 in the times and it'll return the most recent 100h TWAP for the feed. This is useful to not need to construct timestamps for recent TWAPs.&#x20;
