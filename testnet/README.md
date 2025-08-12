# Testnet waypoints

This directory contains the genesis blob and waypoints for Testnet. There are currently two waypoints available:
- `genesis_waypoint.txt`: The genesis waypoint, i.e., the waypoint for the genesis transaction at version `0`.
- `waypoint.txt`: A recent waypoint taken at version `1836949986`, epoch `15870`, on `06/27/2024`.

For operators running Testnet nodes, we recommend using the most recent waypoint. Using the most recent waypoint
will help to improve the security of your node (e.g., by preventing long-range attacks), and allow it to select
the most optimal syncing target when bootstrapping (improving syncing efficiency).

# Submitting framework upgrades on testnet
1. Download the cedra-network Github repo and make sure you're on the main branch: https://github.com/cedra-labs/cedra-network
2. Make sure you have a CLI profile for testnet-voter with the credentials for a voter account corresponding
to a stake pool.
3. To create a proposal on-chain, in the cedra-network repo, run
```
cedra governance propose --is-multi-step \
--metadata-url https://raw.githubusercontent.com/cedra-labs/cedra-networks/main/testnet/proposals/sources/v1.2.3/metadata.json \
--pool-address $pool_address \
--script-path /path/to/cedra-networks/testnet/proposals/sources/v1.2.3/0-move-stdlib.move \
--framework-local-dir /path/to/cedra-network/cedra-move/framework/cedra-framework/ \
--profile testnet-voter
```
4. Verify that the proposal is created on chain.
5. Vote on the proposal with a voter account with at least 100M stake. Use the UI above or run:
```
cedra governance vote --proposal-id <proposal-id> \
--yes \
--profile testnet-voter \
--pool-addresses $pool_address_1,$pool_address_2
```
6. The proposal should become resolved after 30 minutes. To execute a multi-step proposal, execute the following command
once per script (they should be ordered already in the proposal directory):
```
cedra governance execute-proposal --proposal-id <proposal-id> \
--profile testnet-voter \
--script-path path/to/step.move \
--max-gas 500000 \
--framework-local-dir /path/to/cedra-network/cedra-move/framework/cedra-framework/
```
