# CULVERT proving artefacts

The six files a CULVERT holder needs to produce, check or rebuild a spend proof for the
`culvert_T2_L19` circuit (PLONK over BN254, two input legs, two output legs, Merkle depth 19).

**You do not have to trust this repository.** Every file here is pinned by its sha256 in an
immutable contract on BNB Smart Chain, and anyone can check a download against that contract
without asking us anything. If this page disappears, the hashes on chain do not.

## Where to get them

| File | Bytes | IPFS CIDv1 |
|---|---:|---|
| `culvert_T2_L19_plonk.zkey` (proving key) | 81,479,056 | `bafybeidpcanszpc4b2jeebwbq3ctqbzrmddbe44u2xx5oc6xbs7rb2xctq` |
| `culvert_T2_L19.wasm` (witness calculator) | 2,567,806 | `bafybeihjoo37kq2gz7ovtfonu4e2z5siwh6yakwshupazzbw2tlzuroqxa` |
| `culvert_T2_L19.r1cs` | 3,616,088 | `bafybeibe4df75ynbu33beiizppg3kcdqurmftqthj43d2ogcjvjc6nufnu` |
| `culvert_T2_L19_plonk.vkey.json` | 2,044 | `bafkreigovwajkigb6ppiyz22d333i5imd66wk5les3awgqc6rytzqksgye` |
| `PlonkVerifier.sol` (generated verifier source) | 40,012 | `bafkreicxfpz4iec54yurpztb3lvbfkuz3qjiq7m3u6p6bpvr4sqzog7kfy` |
| `ppot_0080_18.ptau` (public phase-1 file, mirrored) | 302,083,218 | `bafybeihe5xxwfdiloonhbfz2rj6mlhms6x66paadjr4koymha6utvu3c7m` |

- **Release assets**: the [`artifacts-culvert_T2_L19`](../../releases/tag/artifacts-culvert_T2_L19)
  release carries all six, plus `SHA256SUMS`, `PINS.txt` and `MANIFEST.json`.
- **IPFS**: any public gateway, for example `https://ipfs.io/ipfs/<CID>`. The CIDs were built with
  `--cid-version=1 --raw-leaves --chunker=size-262144`, so `ipfs add -n` with those flags on your own
  copy reproduces them. Anyone can run `ipfs pin add <CID>` and become an equal mirror.
- **Only a proof needs the first two** (about 80 MiB). The r1cs and the phase-1 file matter only if you
  want to rebuild the proving key yourself; the verification key and the verifier source are here so
  the chain's verifier can be checked, not so it can be trusted.

The phase-1 file is the Perpetual Powers of Tau contribution `ppot_0080_18`, mirrored for
convenience; it is not ours. Its canonical copy is
`https://pse-trusted-setup-ppot.s3.eu-central-1.amazonaws.com/pot28_0080/ppot_0080_18.ptau`.

## How to check a download

```bash
# 1. the bytes are the bytes this list names
shasum -a 256 -c SHA256SUMS

# 2. this list names the bytes the chain pinned
RPC=https://bsc-dataseed.binance.org
NOTE=0xc2DfCe1a58d1F7f3Bb17A31B685EFd77773B15B1           # CulvertNote (cZEC)
ROUTER=$(cast call $NOTE 'VERIFIER()(address)' --rpc-url $RPC)   # 0xb4EF39a1A4485772229b031986D552f75D6dC33F
for PIN in PROVING_KEY_SHA256 WITNESS_WASM_SHA256 R1CS_SHA256 VERIFICATION_KEY_SHA256 VERIFIER_SOURCE_SHA256 PTAU_SHA256; do
  echo "$PIN $(cast call $ROUTER "$PIN()(bytes32)" --rpc-url $RPC)"
done
```

Compare the second list with `PINS.txt`. Every value must be identical. Do step 2 against a note
address you obtained independently: a contract at an address someone else handed you proves only
that the contract says so.

The router's constants are compile-time constants of a contract with no owner, no setter and no
storage for them. They cannot be changed after deployment, by anyone.
