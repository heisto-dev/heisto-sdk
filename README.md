# Heisto SDK

The actual TypeScript instruction encoders, account decoders, PDA derivation, fee calculations and verdict serialization used by the Heisto API. Node 22+ ESM.

## Build and test
```sh
corepack enable
pnpm install --frozen-lockfile
pnpm build
pnpm test
```
This repository is source distribution, not a claim that @heisto/sdk is published on a package registry. Use a reviewed local checkout or a pinned Git commit and build before importing.

## Read-only example after building
```js
import { PublicKey } from '@solana/web3.js';
import { configPda, sentinelPda, currentFee } from './dist/index.js';
const program = new PublicKey('FxqFvpptTRCeKCxCuxt86AJze6v16Vk9M8UZCQ8tJvbi');
const owner = new PublicKey('CvAHQN2RuY3d1C4Bh2qm5JfUnpLXUrC1amk3AnDpFdgB');
console.log(configPda(program).toBase58());
console.log(sentinelPda(program, owner, 3544635135356959125n).toBase58());
console.log(currentFee(1000000n, 0n).toString());
```
These are public example addresses. No signer is loaded, no transaction is broadcast and the ID is a BigInt to avoid precision loss.

## Exports
- `LAMPORTS_PER_SOL`
- `MIN_POOL_LAMPORTS`
- `WIN_SCORE`
- `STATUS`
- `STATUS_NAME`
- `ixDisc`
- `accDisc`
- `configPda`
- `programDataPda`
- `sentinelPda`
- `attackPda`
- `currentFee`
- `mulBps`
- `verdictMessage`
- `ix`
- `ConfigAccount`
- `decodeConfig`
- `SentinelAccount`
- `decodeSentinel`
- `AttackAccount`
- `decodeAttack`
- `encodeSentinel`
- `encodeAttack`
- `encodeConfig`
- `lamportsToSol`
- `solToLamports`

## Conventions and safety
- All integer monetary values are lamports (1 SOL = 1,000,000,000 lamports); u64 IDs/counters use BigInt.
- Anchor discriminators are the first eight SHA-256 bytes of the account/instruction name domain.
- Sentinel seeds: UTF-8 sentinel, 32-byte owner public key, u64 little-endian ID.
- Attack seeds: UTF-8 attack, Sentinel public key, u64 little-endian sequence.
- Verdict domain is HEISTO_V1. Message length is 116 bytes.
- ix constructs instructions, not authorization. Validate network, signer, accounts, amount, fee, program, blockhash and simulation before signing.
- Tests generate ephemeral test keys. No funded private identity or production signing key is included.

See [architecture and flows](https://github.com/heisto-dev/heisto-docs) and [Mainnet proof](https://github.com/heisto-dev/heisto-mainnet-proof). No independent audit is claimed. MIT; support/security: support@heisto.xyz.
