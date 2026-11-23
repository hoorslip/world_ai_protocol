# world_ai_protocol

AI agents need permission to act on-chain without unrestricted access to a user's account. world_ai_protocol provides a delegation model for function-level access, time limits, and call allowances. It targets Move-based Web3 integrations that need AI agent authorization, with a chain-agnostic permission model.

## Features

- Grant access to individual functions and bind each agent to one principal at a time.
- Set start and end timestamps for access; a zero value leaves that boundary unrestricted.
- Limit calls per authorized function and remove that permission when its allowance reaches zero.
- Update time bounds or call allowances, revoke individual permissions, and clear the principal association when no permissions remain.
- Require an agent signature as consent when creating an authorization.
- Create or update multiple authorizations in one transaction and emit events for authorization changes.

## Installation

Install the Sui CLI for a Sui-based Move checkout. Other Move environments require their own toolchain; the chain-agnostic design does not make deployment commands interchangeable.

The repository owner and release assets for this standalone project are not specified. Replace `OWNER` with the publishing account. Run the build commands from the directory that contains `Move.toml`.

```sh
git clone https://github.com/OWNER/world_ai_protocol.git
cd world_ai_protocol
sui move build
sui move test
```

The release download below assumes the publisher provides an asset named `world_ai_protocol-v0.1.0.tar.gz` under the `v0.1.0` release. Its availability is not confirmed.

```sh
curl -fL https://github.com/OWNER/world_ai_protocol/releases/download/v0.1.0/world_ai_protocol-v0.1.0.tar.gz -o world_ai_protocol-v0.1.0.tar.gz
```

## Usage

A principal grants an agent access to a particular function, supplies the agent's consent signature, and sets the time window and call allowance. Protected calls check those permissions before execution. Each successful call consumes one allowance; the principal can revoke access before the allowance runs out.

**Move integration is chain-specific.** The source material does not establish a published package ID, Move module names, or entry-function signatures. The following is a Sui invocation template, not a verified contract API. Replace the module, function, and arguments with those declared by your deployed package.

```sh
# Illustrative argument order only:
# authorization state, agent, target function, start, end, allowance, consent signature
sui client call \
  --package "$PACKAGE_ID" \
  --module "$AUTH_MODULE" \
  --function "$AUTHORIZE_FUNCTION" \
  --args "$AUTH_STATE_ID" "$AGENT_ADDRESS" "$TARGET_FUNCTION" \
    0 0 10 "$AGENT_SIGNATURE" \
  --gas-budget 100000000
```

In this example, both time boundaries are unrestricted and the requested allowance is ten calls. Use the identifier format required by the Move implementation for the target function; the donor's EVM function selectors are not a universal Move interface.

Verify that every protected application function checks authorization. Recording a delegation alone does not restrict an application's entry points.

## License

Licensed under Apache-2.0. This project is based on [WORLD3-ai/world_ai_protocol](https://github.com/WORLD3-ai/world_ai_protocol). Preserve applicable copyright and attribution notices, include the license, and retain any required NOTICE content when redistributing derived code. Mark modified files as changed.

## Related projects

Other repositories from the same author, as supplied with this project brief. Their functionality was not provided; consult their READMEs for scope and usage.

- [priyez/kashti](https://github.com/priyez/kashti): A separate repository; its purpose is not documented here.
- [21Seni/skan](https://github.com/21Seni/skan): A separate repository; no integration with this protocol is specified.
- [metmsipha-bit/python-vpn](https://github.com/metmsipha-bit/python-vpn): A separate repository; its supported workflows are not documented here.

