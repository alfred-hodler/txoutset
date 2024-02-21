# UTXO Set Parser

Bitcoin Core produces a large binary file containing a compressed representation of the UTXO set via the `dumptxoutset` RPC call. This library contains data structures to parse and decompress the resulting dump file.

## Creating a UTXO Snapshot with Bitcoin Core

Generating the snapshot takes a few minutes and requires more than 10 GB.

```shell
$ bitcoin-cli dumptxoutset /tmp/utxo.dat
{
  "coins_written": 161672056,
  "base_hash": "0000000000000000000165eb2d431a3c8c798b251d02ad64642de413adf67277",
  "base_height": 830610,
  "path": "/tmp/utxo.dat",
  "txoutset_hash": "c6e8ea01a867bd31aacaeae613ad07be0730f3ad67db6abe626469ce3e32169b",
  "nchaintx": 965912411
}
```

## Parsing with Example Program

Check for basic parsing of the dump file.

```shell
$ cargo run -p txoutset-csv -- -c /tmp/utxo.dat
Dump opened.
 Block Hash: 0000000000000000000165eb2d431a3c8c798b251d02ad64642de413adf67277
 UTXO Set Size: 161672056
```

Parse entries and use in pipeline. In this example, we find coinbase outputs.

```shell
$ cargo run -p txoutset-csv -- -a /tmp/utxo.dat | grep ":0,1," | head -n 10
f14eb050a182d451b360bbb14386e1a28dd067e873b660b3187d55eae4b30200:0,1,84990,5000000000,41047a488354d9d5414de09b7121b80b973c991b76998ad68756d8cf4560c0ddcbe24ae72a77cca86f6d1d11b1b796e3f14caa5852e6d4c60e9bebedc71673b58ec0ac,
3c6f38c5631452503071cabe3688063b107f7e60251dc2d709fc269448f30600:0,1,23354,5000000000,4104491dcb599964d1a0656aaa8c78775f446111b923c42347f9d8e76c3ddf049371bbafc3cd390edd087e4551cf6dc90e8fa46ee95eacaf8928765b38d35ae8c736ac,
33a319c7bed6b591286a58d06f27270f8bdcd0ece5ad33e20c96f4a2487d0900:0,1,17564,5000000000,41046de49633185c9dd2fdd224c99f5efbc2254ba347dd6b38e58c43a16ac2821d7aa1a390de4de769716073e56e030d3b1e19a356680f7d54c309ca1c12c2e9df26ac,
b2cb3386393044be02e167bed12bb2664218b9c0c3c2d6465f48a51a73c10900:0,1,702122,640198554,00143156afc4249915008020f932783319f3e610b97d,bc1qx9t2l3pyny2spqpqlye8svce70nppwtaxwdrp4
4b46ca82e1e5d156f505a7003478c29d3fdf90ec0e1e7e9019e545087fe10900:0,1,2039,5000000000,4104ba3f89b5b9a9d3e8ccc43a0cf45a8dcb4664d84433763a3d968e79d7f98d33de184bcef90b02dd0ead7e863e6a2d4c29970d90623f05f7008e468bb0750252c6ac,
31404d605a8aedb7bb0012fb71a7b7006d455876b76fe167b2cfcaf9d27d0a00:0,1,163614,5000000000,4104809bf43e256ebb872860de54f19915c9e51f91e57c0837bd2d66adc34791b2c2f85a788b75d5d177393028e9961dde2bb4f76c23282610cebe292555dfb4d668ac,
185e12c264c7748ea68c744241e2a01ce19afee553b4dd55b10c7aa47cb40a00:0,1,291349,1554990,76a914dfcaee51f3c39f00876c7f0e92f252a6948ca22588ac,1MQJnfgbZNsV6UCBgApWtkgDGdzePoWqRv
9aa7eb4e5f6d9a0da14952a5214b6790eea6fc9ffa2ca8061a7173aee14e0b00:0,1,17222,5000000000,410437de5778e47b3d73a64420bd73476c9b974bf8d012a18969276d3a3ade68750ad65a52e329fa1a0af7c6a63ea609f09d235f28b64a193549d90644b5cb388decac,
1ec85cd38a92288b7ba007d2fd338c5074991ef92023555732b7b020c5960c00:0,1,826881,546,76a914c6740a12d0a7d556f89782bf5faf0e12cf25a63988ac,1K6KoYC69NnafWJ7YgtrpwJxBLiijWqwa6
05ad903b80c9c95ddb5cbdf7a56c8e39becfbb49c73ff95826b6f5c649a60d00:0,1,72061,5000000000,4104b46eaa0f981b73bef8b9ff74b6235d9ad14f501acf5281ac2e9d40d6543da7eae6cfee2d98d6023bd6e390a504a5558c9b7da31a913d9ac5fdeeeff86f08971aac,
```
