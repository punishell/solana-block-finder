# Solana Block Finder

This Rust tool finds the latest Solana block that matches a given timestamp or is right before that timestamp if no exact match exists.

## Features

- Uses binary search to efficiently find blocks by timestamp
- Handles slots with missing timestamps
- Provides detailed block information including block hash, height, and time
- Shows the time difference between the requested timestamp and found block

## Prerequisites

- Rust and Cargo installed
- A Helius API key (a free one is provided by default)

## Installation

1. Clone this repository
2. Build the project:

```bash
cargo build --release
```

## Usage

```bash
./target/release/solana-block-finder -t <timestamp> -k [api_key]
```

Parameters:
- `timestamp`: Unix timestamp in seconds
- `api_key`: (Optional) Your Helius API key. If not provided, it will try to use the `HELIUS_API_KEY` environment variable or fall back to the default key.

### Basic Example:
```bash
./target/release/solana-block-finder -t 1650000000 -k API_KEY
```

### Verification Example

Here's a real-world verification using block data from the [Solana Explorer](https://explorer.solana.com/):

**Block Explorer Data:**
- **Block:** [349,274,161](https://explorer.solana.com/block/349274161)
- **Blockhash:** `5yZQFCS5BjhjNeCpCcgwLKwAR2xMZYEvk5wyYxximxMs`
- **Timestamp (UTC):** Jun 26, 2025 at 07:10:05 UTC
- **Unix Timestamp:** `1750921805`

**Command:**
```bash
./target/release/solana-block-finder -t 1750921805 -k ...
```

**Output:**
```
Searching for block with timestamp 1750921805 or right before it...
Current slot: 349274812
Starting binary search for timestamp: 1750921805
Slot 174637406 has timestamp 1674812766
Slot 261956109 has timestamp 1713952417
Slot 305615461 has timestamp 1733425581
Slot 327445137 has timestamp 1742251579
Slot 338359975 has timestamp 1746593284
Slot 343817394 has timestamp 1748747961
Slot 346546103 has timestamp 1749827127
Slot 347910458 has timestamp 1750373619
Slot 348592635 has timestamp 1750648092
Slot 348933724 has timestamp 1750785379
Slot 349104268 has timestamp 1750854022
Slot 349189540 has timestamp 1750888038
Slot 349232176 has timestamp 1750905047
Slot 349253494 has timestamp 1750913557
Slot 349264153 has timestamp 1750917814
Slot 349269483 has timestamp 1750919936
Slot 349272148 has timestamp 1750921005
Slot 349273480 has timestamp 1750921534
Slot 349274146 has timestamp 1750921800
Slot 349274479 has timestamp 1750921936
Slot 349274312 has timestamp 1750921866
Slot 349274229 has timestamp 1750921832
Slot 349274187 has timestamp 1750921816
Slot 349274166 has timestamp 1750921807
Slot 349274156 has timestamp 1750921804
Slot 349274161 has timestamp 1750921805

Found block:
Slot: 349274161
Block hash: 5yZQFCS5BjhjNeCpCcgwLKwAR2xMZYEvk5wyYxximxMs
Block time: 1750921805
Block height: 327476800
This block exactly matches the requested timestamp.
```

**Verification Results:** ✅
- ✅ **Exact Match:** Found the precise slot `349274161` 
- ✅ **Correct Timestamp:** `1750921805` matches exactly
- ✅ **Verified Blockhash:** Matches the [Solana Explorer data](https://explorer.solana.com/block/349274161)
- ✅ **Efficient Search:** Binary search navigated through millions of slots in ~25 API calls

## How It Works

The tool uses a binary search algorithm to efficiently find the block with the timestamp closest to the requested one:

1. Gets the current slot from the Solana network
2. Performs a binary search between slot 0 and the current slot
3. For each slot in the search, fetches its timestamp
4. Handles slots with missing timestamps by checking nearby slots
5. Returns the slot with the timestamp closest to but not exceeding the requested timestamp
6. Fetches and displays detailed information about the found block

## Future Improvements

Given more time, the following improvements could be made:

1. Add support for devnet and testnet
2. Implement caching to reduce API calls
3. Add more error handling and retry logic for network failures
4. Create a more user-friendly output format (JSON, CSV)
5. Add support for finding blocks by other criteria
6. Implement parallel processing for faster searches
7. Add unit and integration tests
8. Create a web interface or API endpoint
9. Optimize the binary search algorithm further
10. Add support for batch processing multiple timestamps 
