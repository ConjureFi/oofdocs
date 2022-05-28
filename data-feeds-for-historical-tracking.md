# Data Feeds for Historical Tracking

OOF also supports tools for retrieving historical data, each feed has a timeslot (a value which indicates how often to update the feed and how long an update will be valid for submits) which saves the price at a given time. If the timeslot has a price that was submitted during that timeslot, then that will be returned by the contract when the user calls

historicalFeeds(uint256\[] feedIDs, uint256\[] timestamps)

This allows users to simply input a timestamp without needing to worry about which timeslot it relates to and can get a historical value for that feed at that time, for the history of the whole feeds lifespan. This allows for analytics as well as TWAP construction around these feeds.
