# Scaling Bitcoin Oracle - "on demand" data model



## Consensus layer for all and any off-chain computation engines

Bitcoin Oracle acts as a consensus layer for all and any off-chain computation engines that relies on the Bitcoin data.

The consensus is reached when a minimum threshold (i.e. "_m-of-n_") of the relevant community agree to a particular event.

End consumers can then verify the consensus before making a decision or taking an action with respect to that particular event, thus enhancing the security assumptions.

For example, Bitcoin Oracle is used by Xlink for their [BRC20 Bridge](https://app.xlink.network/bridge/brc20-bridge/peg-in), which uses a [smart contract on Stacks](https://explorer.hiro.so/txid/SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.btc-bridge-endpoint-v1-10?chain=mainnet) to verify the consensus and save the validated data on-chain.

When Xlink identifies a particular BRC20 transfer to process, it pulls the relevant consensus data from Bitcoin Oracle, verify them using its smart contract before saving them on-chain.

This combination of the off-chain computation and the on-chain verification protects Xlink users from a potential security issues associated with BRC20, including that of [double spend](https://en.wikipedia.org/wiki/Double-spending).

## Enc consumer of consensus data dictates how it is verified

An important point to note is that, while Bitcoin Oracle produces the consensus data based on the computation from the off-chain indexers, the verification of such consensus data can be implemented by the end consumer as they see fit.

For example, wallet integrating Bitcoin Oracle may implement a client-side verification of the consensus data, instead of relying on an on-chain verification.

A website that wants to display users' BRC20 balances, but do not need to verify the consensus data, may simply use the data without verification.

A BRC20 trading dapp on an EVM-compatible Bitcoin L2 may implement their own smart contract written in Solidity to verify the consensus data, before initiating other smart contract interactions.

## So how can one fetch the consensus data from Bitcoin Oracle?

### Summary data

If you need only the summary data, without its associated consensus data, you can use the following endpoints at [https://api.alexgo.io](https://api.alexgo.io/).

{% swagger src="https://api.alexgo.io/swagger-ui-yaml" path="/v1/bitcoin-oracle/user-balance" method="get" expanded="true" %}
[https://api.alexgo.io/swagger-ui-yaml](https://api.alexgo.io/swagger-ui-yaml)
{% endswagger %}

{% swagger src="https://api.alexgo.io/swagger-ui-yaml" path="/v1/bitcoin-oracle/bitcoin-tx-indexed" method="get" %}
[https://api.alexgo.io/swagger-ui-yaml](https://api.alexgo.io/swagger-ui-yaml)
{% endswagger %}

### Full consensus data

If you need the full consensus data, you can use the following endpoint at [https://api.bitcoin-oracle.network](https://api.bitcoin-oracle.network/).

The endpoint performs a match against an array where each item represents a distinct condition. If multiple query parameters are specified, such as `from` and `height`, the returned transactions must satisfy **all** the given conditions.&#x20;

The endpoint supports query conditions for addresses in two formats: Bech32 and PKScript.

{% swagger src="https://api.bitcoin-oracle.network/swagger-ui-yaml" path="/v1/brc20" method="post" %}
[https://api.bitcoin-oracle.network/swagger-ui-yaml](https://api.bitcoin-oracle.network/swagger-ui-yaml)
{% endswagger %}

#### Examples

{% tabs %}
{% tab title="Historical transfer by Bitcoin {tx_id}" %}


**Body**

```
{
    "type": "tx_id",
    "tx_id":"9778d139d55f3dca60e4d45942cf6c0ab97e23fd24771696b9ecdfba52303b01",
    "output": 0,
    "offset": 0
}
```

**Response**

```
{
    "data": [
        {
            "tx_hash": "020000000001022f119e4f00a1ea91771cd8dfc8e459cf98b7aac138aef6f4ff9394f53ab28faf0000000000fdffffff3f90f44767460ec0a742fe5320238cd05d8fbd3d292768ea5d5b38b29c7744760100000000fdffffff0322020000000000002251207e875ae46c9f2d28d8d302cfa8c045bfc94df9277dbdf2d7a1fcdba1297318998e0902000000000016001412c625feb11e662805f4923b289a15d6be803ed70000000000000000386a360c0000000204646573740100000000000000000000000000000000047573657205166f4c0cb79258561dddf49ad4b91479324935b0dd01403a09be942697dc269ae4766a2a885911fd477306cd3d5dfad03cad5377d887a7b59c0bf78734df40cd6d44940a20f5f02896911a43231e81b7ee864311cd8cd202473044022035237c80120014629af1b1f1db73f6ed74e748a5afd398e05ce35a81e56130f902204ef75df72cce7fc124ce4696cb8155b5dfad16950eea1c7de0eedc761ffa7ff00121025ce5bd977e990a1e653519333c35fa2d7531a1b5cc6737e2057165ff148aca5b00000000",
            "order_hash": "337d20f2431441591a46f8c65569e5e8bea98c70e0d6fa243ed19c4b40c3f9b7",
            "amt": "3700000000000000000000000",
            "bitcoin_tx": "00000020e7f505d8d9098cca86d39b4889f4ae4d2514929e16b0030000000000000000008d1d3bbee8ca41c87cea86e0d1eef44789a34051b3b48c68c4f358d488e75de8948e9f6569d80317921bb034",
            "decimals": "18",
            "from": {
                "pkscript": "5120aa9145519b90c95b6b41044bd7c63ea661bdb6c0bdbda590a4cb759f9978915b",
                "address": "bc1p42g525vmjry4k66pq39a03375esmmdkqhk76ty9yed6elxtcj9ds9pm7dt"
            },
            "from_bal": "0",
            "offset": "0",
            "output": "0",
            "tick": "ORMM",
            "to": {
                "pkscript": "51207e875ae46c9f2d28d8d302cfa8c045bfc94df9277dbdf2d7a1fcdba129731899",
                "address": "bc1p06r44ervnukj3kxnqt863sz9hly5m7f80k7l94aplnd6z2tnrzvstdkzsq"
            },
            "to_bal": "612741501250300100000000000",
            "tx_id": "9778d139d55f3dca60e4d45942cf6c0ab97e23fd24771696b9ecdfba52303b01",
            "proof_hashes": [
                "0048801e960dbf63c4602968259b23efc84d01b56f765c748d02b7387fda083c",
                "c2ad185f79ab5e04def0b4ae8a1b971f03d738501c999be7b04d453f115d0c99",
                "dde8ca635a80f4978049d95e53621f85b81a109e739c8a17e3a87589baff3378",
                "3f0a3723a1141af85387d1180c5d25fd5c0ba1de9d140d46b608511659d146e1",
                "02c2fe271149f95e3b2ce44441158c0e6459df5a75297df6d3e4733d78ee5577",
                "223866a29166b32436d84381dea03193a1fddcabe93d5752a05d42539c754641",
                "b58fd40d5861a7a1407315f2d91e328442fa25f4a9f2dc7a937aff2e05fb0c24",
                "f2d8b750b230ea7a05ab8c8f9c607410bfa8c85b1066128be835cd7f3fe3e787",
                "190320c66700b5bf8decea0a29491b5c3a0f53692823107dbe01b99af615c263",
                "fa3e58599aa6573f3702bf0d53bd306f57713a642b696142131b6e009257d600",
                "ecf791c893b78c40c2aad6125aea4ee5dd39d92a4ecbd421a9b38c90433dd3d3",
                "2c202c28b430c7b7b726b006c55a661af87fdcb349ded3becc5d563baa39cf8a"
            ],
            "tx_index": "2049",
            "tree_depth": "12",
            "height": "825254",
            "signers": [
                "SP1B0DHZV858RCBC8WG1YN5W9R491MJK88QPPC217",
                "SP35D14R1JB3KHE4D55MMN646GFZJ198B7FMBG5PD",
                "SP3ZCGVGSQWT6TJAXZRXHCYA6SWGNSNGKHSYA1F2Q"
            ],
            "signer_types": [
                "bis",
                "hiro",
                "unisat"
            ],
            "signatures": [
                "0fc66c9a7fe85ce78740fcc07f60ac47d5a05c469b2ca5f1ee16f5cc4b6eda1e41367bd67b03e7fbc6e4ba883c6fad1551373bd786f59afefbed268d08e59fd500",
                "cd63d0ffae136ca1ffe1681ec4b062ada335b002b96b2a6153cea0a1f3ed94f14b601202ac2c8ecb229168c1264d0fad3475307df5a493ad98b9fae431f5138401",
                "f7045fca2269e91d632f4a259eb47cc9f3ebb17dd64f7b8f4dfc8aa2ecfb2e716bfcce5f3c8ec6be0987c6f620d8c4205cd8d0c9728ee73c8892768ffca211c101"
            ],
            "created_at": 1704970783160,
            "updated_at": 1704970783160
        }
    ]
}
```
{% endtab %}

{% tab title="Historical transfer of {tick} from {from} to {to}" %}


**Body**

```
{
    "from": [
        "bc1p42g525vmjry4k66pq39a03375esmmdkqhk76ty9yed6elxtcj9ds9pm7dt"
    ],
    "to": [
        "bc1p06r44ervnukj3kxnqt863sz9hly5m7f80k7l94aplnd6z2tnrzvstdkzsq"
    ],
    "tick": [
        "ORMM"
    ],
    "limit": 10
}
```

**Response**

```
{
    "data": [
        {
            "tx_hash": "020000000001022f119e4f00a1ea91771cd8dfc8e459cf98b7aac138aef6f4ff9394f53ab28faf0000000000fdffffff3f90f44767460ec0a742fe5320238cd05d8fbd3d292768ea5d5b38b29c7744760100000000fdffffff0322020000000000002251207e875ae46c9f2d28d8d302cfa8c045bfc94df9277dbdf2d7a1fcdba1297318998e0902000000000016001412c625feb11e662805f4923b289a15d6be803ed70000000000000000386a360c0000000204646573740100000000000000000000000000000000047573657205166f4c0cb79258561dddf49ad4b91479324935b0dd01403a09be942697dc269ae4766a2a885911fd477306cd3d5dfad03cad5377d887a7b59c0bf78734df40cd6d44940a20f5f02896911a43231e81b7ee864311cd8cd202473044022035237c80120014629af1b1f1db73f6ed74e748a5afd398e05ce35a81e56130f902204ef75df72cce7fc124ce4696cb8155b5dfad16950eea1c7de0eedc761ffa7ff00121025ce5bd977e990a1e653519333c35fa2d7531a1b5cc6737e2057165ff148aca5b00000000",
            "order_hash": "337d20f2431441591a46f8c65569e5e8bea98c70e0d6fa243ed19c4b40c3f9b7",
            "amt": "3700000000000000000000000",
            "bitcoin_tx": "00000020e7f505d8d9098cca86d39b4889f4ae4d2514929e16b0030000000000000000008d1d3bbee8ca41c87cea86e0d1eef44789a34051b3b48c68c4f358d488e75de8948e9f6569d80317921bb034",
            "decimals": "18",
            "from": {
                "pkscript": "5120aa9145519b90c95b6b41044bd7c63ea661bdb6c0bdbda590a4cb759f9978915b",
                "address": "bc1p42g525vmjry4k66pq39a03375esmmdkqhk76ty9yed6elxtcj9ds9pm7dt"
            },
            "from_bal": "0",
            "offset": "0",
            "output": "0",
            "tick": "ORMM",
            "to": {
                "pkscript": "51207e875ae46c9f2d28d8d302cfa8c045bfc94df9277dbdf2d7a1fcdba129731899",
                "address": "bc1p06r44ervnukj3kxnqt863sz9hly5m7f80k7l94aplnd6z2tnrzvstdkzsq"
            },
            "to_bal": "612741501250300100000000000",
            "tx_id": "9778d139d55f3dca60e4d45942cf6c0ab97e23fd24771696b9ecdfba52303b01",
            "proof_hashes": [
                "0048801e960dbf63c4602968259b23efc84d01b56f765c748d02b7387fda083c",
                "c2ad185f79ab5e04def0b4ae8a1b971f03d738501c999be7b04d453f115d0c99",
                "dde8ca635a80f4978049d95e53621f85b81a109e739c8a17e3a87589baff3378",
                "3f0a3723a1141af85387d1180c5d25fd5c0ba1de9d140d46b608511659d146e1",
                "02c2fe271149f95e3b2ce44441158c0e6459df5a75297df6d3e4733d78ee5577",
                "223866a29166b32436d84381dea03193a1fddcabe93d5752a05d42539c754641",
                "b58fd40d5861a7a1407315f2d91e328442fa25f4a9f2dc7a937aff2e05fb0c24",
                "f2d8b750b230ea7a05ab8c8f9c607410bfa8c85b1066128be835cd7f3fe3e787",
                "190320c66700b5bf8decea0a29491b5c3a0f53692823107dbe01b99af615c263",
                "fa3e58599aa6573f3702bf0d53bd306f57713a642b696142131b6e009257d600",
                "ecf791c893b78c40c2aad6125aea4ee5dd39d92a4ecbd421a9b38c90433dd3d3",
                "2c202c28b430c7b7b726b006c55a661af87fdcb349ded3becc5d563baa39cf8a"
            ],
            "tx_index": "2049",
            "tree_depth": "12",
            "height": "825254",
            "signers": [
                "SP1B0DHZV858RCBC8WG1YN5W9R491MJK88QPPC217",
                "SP35D14R1JB3KHE4D55MMN646GFZJ198B7FMBG5PD",
                "SP3ZCGVGSQWT6TJAXZRXHCYA6SWGNSNGKHSYA1F2Q"
            ],
            "signer_types": [
                "bis",
                "hiro",
                "unisat"
            ],
            "signatures": [
                "0fc66c9a7fe85ce78740fcc07f60ac47d5a05c469b2ca5f1ee16f5cc4b6eda1e41367bd67b03e7fbc6e4ba883c6fad1551373bd786f59afefbed268d08e59fd500",
                "cd63d0ffae136ca1ffe1681ec4b062ada335b002b96b2a6153cea0a1f3ed94f14b601202ac2c8ecb229168c1264d0fad3475307df5a493ad98b9fae431f5138401",
                "f7045fca2269e91d632f4a259eb47cc9f3ebb17dd64f7b8f4dfc8aa2ecfb2e716bfcce5f3c8ec6be0987c6f620d8c4205cd8d0c9728ee73c8892768ffca211c101"
            ],
            "created_at": 1704970783160,
            "updated_at": 1704970783160
        }
    ]
}
```
{% endtab %}

{% tab title="Batch historical transfer (multiple {tick}, {from} and/or {to})" %}


**Body**

```
{
    "from": [
        "bc1p42g525vmjry4k66pq39a03375esmmdkqhk76ty9yed6elxtcj9ds9pm7dt",
        "bc1pgpxqrl8gcykc970kkczswaau4ur4rcjat9y0pvmagkfhhjddzscq6mdpf4"
    ],
    "to": [
        "bc1p06r44ervnukj3kxnqt863sz9hly5m7f80k7l94aplnd6z2tnrzvstdkzsq"
    ],
    "tick": [
        "ORMM",
        "OXBT"
    ],
    "limit": 100
}
```

**Response**

```
{
    "data": [
        {
            "tx_hash": "020000000001022f119e4f00a1ea91771cd8dfc8e459cf98b7aac138aef6f4ff9394f53ab28faf0000000000fdffffff3f90f44767460ec0a742fe5320238cd05d8fbd3d292768ea5d5b38b29c7744760100000000fdffffff0322020000000000002251207e875ae46c9f2d28d8d302cfa8c045bfc94df9277dbdf2d7a1fcdba1297318998e0902000000000016001412c625feb11e662805f4923b289a15d6be803ed70000000000000000386a360c0000000204646573740100000000000000000000000000000000047573657205166f4c0cb79258561dddf49ad4b91479324935b0dd01403a09be942697dc269ae4766a2a885911fd477306cd3d5dfad03cad5377d887a7b59c0bf78734df40cd6d44940a20f5f02896911a43231e81b7ee864311cd8cd202473044022035237c80120014629af1b1f1db73f6ed74e748a5afd398e05ce35a81e56130f902204ef75df72cce7fc124ce4696cb8155b5dfad16950eea1c7de0eedc761ffa7ff00121025ce5bd977e990a1e653519333c35fa2d7531a1b5cc6737e2057165ff148aca5b00000000",
            "order_hash": "337d20f2431441591a46f8c65569e5e8bea98c70e0d6fa243ed19c4b40c3f9b7",
            "amt": "3700000000000000000000000",
            "bitcoin_tx": "00000020e7f505d8d9098cca86d39b4889f4ae4d2514929e16b0030000000000000000008d1d3bbee8ca41c87cea86e0d1eef44789a34051b3b48c68c4f358d488e75de8948e9f6569d80317921bb034",
            "decimals": "18",
            "from": {
                "pkscript": "5120aa9145519b90c95b6b41044bd7c63ea661bdb6c0bdbda590a4cb759f9978915b",
                "address": "bc1p42g525vmjry4k66pq39a03375esmmdkqhk76ty9yed6elxtcj9ds9pm7dt"
            },
            "from_bal": "0",
            "offset": "0",
            "output": "0",
            "tick": "ORMM",
            "to": {
                "pkscript": "51207e875ae46c9f2d28d8d302cfa8c045bfc94df9277dbdf2d7a1fcdba129731899",
                "address": "bc1p06r44ervnukj3kxnqt863sz9hly5m7f80k7l94aplnd6z2tnrzvstdkzsq"
            },
            "to_bal": "612741501250300100000000000",
            "tx_id": "9778d139d55f3dca60e4d45942cf6c0ab97e23fd24771696b9ecdfba52303b01",
            "proof_hashes": [
                "0048801e960dbf63c4602968259b23efc84d01b56f765c748d02b7387fda083c",
                "c2ad185f79ab5e04def0b4ae8a1b971f03d738501c999be7b04d453f115d0c99",
                "dde8ca635a80f4978049d95e53621f85b81a109e739c8a17e3a87589baff3378",
                "3f0a3723a1141af85387d1180c5d25fd5c0ba1de9d140d46b608511659d146e1",
                "02c2fe271149f95e3b2ce44441158c0e6459df5a75297df6d3e4733d78ee5577",
                "223866a29166b32436d84381dea03193a1fddcabe93d5752a05d42539c754641",
                "b58fd40d5861a7a1407315f2d91e328442fa25f4a9f2dc7a937aff2e05fb0c24",
                "f2d8b750b230ea7a05ab8c8f9c607410bfa8c85b1066128be835cd7f3fe3e787",
                "190320c66700b5bf8decea0a29491b5c3a0f53692823107dbe01b99af615c263",
                "fa3e58599aa6573f3702bf0d53bd306f57713a642b696142131b6e009257d600",
                "ecf791c893b78c40c2aad6125aea4ee5dd39d92a4ecbd421a9b38c90433dd3d3",
                "2c202c28b430c7b7b726b006c55a661af87fdcb349ded3becc5d563baa39cf8a"
            ],
            "tx_index": "2049",
            "tree_depth": "12",
            "height": "825254",
            "signers": [
                "SP1B0DHZV858RCBC8WG1YN5W9R491MJK88QPPC217",
                "SP35D14R1JB3KHE4D55MMN646GFZJ198B7FMBG5PD",
                "SP3ZCGVGSQWT6TJAXZRXHCYA6SWGNSNGKHSYA1F2Q"
            ],
            "signer_types": [
                "bis",
                "hiro",
                "unisat"
            ],
            "signatures": [
                "0fc66c9a7fe85ce78740fcc07f60ac47d5a05c469b2ca5f1ee16f5cc4b6eda1e41367bd67b03e7fbc6e4ba883c6fad1551373bd786f59afefbed268d08e59fd500",
                "cd63d0ffae136ca1ffe1681ec4b062ada335b002b96b2a6153cea0a1f3ed94f14b601202ac2c8ecb229168c1264d0fad3475307df5a493ad98b9fae431f5138401",
                "f7045fca2269e91d632f4a259eb47cc9f3ebb17dd64f7b8f4dfc8aa2ecfb2e716bfcce5f3c8ec6be0987c6f620d8c4205cd8d0c9728ee73c8892768ffca211c101"
            ],
            "created_at": 1704970783160,
            "updated_at": 1704970783160
        },
        {
            "tx_hash": "020000000001023c99d5e25fac74e9d73d48f524e4a4772fcff9b6515d4383a5b4de1f93734f590000000000fdffffffb5c50d3e99ad5cab208c154047ae24290de2895ff68b19e8237f19ed707d02a30100000017160014df456fbaa7c5f34de88cc94494c14f0d6e83f0b2fdffffff0322020000000000002251207e875ae46c9f2d28d8d302cfa8c045bfc94df9277dbdf2d7a1fcdba1297318999b873b000000000017a9147677a8e87a0b9fdaffcf2837bbfddd82adc7bcbd870000000000000000386a360c0000000204646573740100000000000000000000000000000000047573657205164e928c9b9f2884e66ebc9cffa24146da5c4c35450140f7b6b87eea8059209ec74c7418a59050bd0bd56cdded5cde1e684646628d9b8864803b32d6f5eec08c65887ae065f58687484ca5aad9eb5fcfdbabfa092afced02483045022100c2ea597e0b0948812dccb4cf7ab9700bf6d335d85665dce9d1d1cc729bfeaf1302205f8071e8d6c3ad136cc9543c65ff7ebc927b13955b8b462ffeb71f7eac28c1f2012102877a31bb403b5f261df23397709629e6cc2aa4b33a5d915263dd1b776f4d9d4800000000",
            "order_hash": "4576d334224e0e693377d120253ff2bcb80659b523d5e4ba9d240110cf82cd84",
            "amt": "24500000000000000000000",
            "bitcoin_tx": "0000ff3f498d61c4022daa78511e5813c26548790374795d314e030000000000000000006bf1aaef26015cdfbaab2dbfc90559d16b3323bc2faa8741457cbe9668fd145c025e9f6569d803172c5b5c1a",
            "decimals": "18",
            "from": {
                "pkscript": "5120404c01fce8c12d82f9f6b6050777bcaf0751e25d5948f0b37d45937bc9ad1430",
                "address": "bc1pgpxqrl8gcykc970kkczswaau4ur4rcjat9y0pvmagkfhhjddzscq6mdpf4"
            },
            "from_bal": "0",
            "offset": "0",
            "output": "0",
            "tick": "OXBT",
            "to": {
                "pkscript": "51207e875ae46c9f2d28d8d302cfa8c045bfc94df9277dbdf2d7a1fcdba129731899",
                "address": "bc1p06r44ervnukj3kxnqt863sz9hly5m7f80k7l94aplnd6z2tnrzvstdkzsq"
            },
            "to_bal": "39776330400000000000000",
            "tx_id": "08fc19fd8f6d9d51b749ab749681b73cc4f509017e93ba2e8a2057067fbe03af",
            "proof_hashes": [
                "b05924a5ebc27daf9e4c098c87cfd221be41eae11e6f1f7f7077fc8cdab920a1",
                "bd2c2e28b685e32b87df545df150fbb365ffc6f4c0212e5e91825ffc68ee3b7b",
                "cd90c2c8253afe98f9b4b508fe5de98183455dc3c2b038800b6d79392d7ff631",
                "35ccbc548dadd37c52a3e2dc5b3e3368b42138eee021b15116c4d28a738b3783",
                "3849fc4ef7251102c0455ab8f93d515f07a169bc59c5bc73adcbbbfb9bf6a4f3",
                "886d7f840b46fce7deadfe9ab284f01de2ef0aaa02d203bba79988b297aca71e",
                "b6819566d285ee1425b5228a22b8ec17866adfc997cc49fdefafd48164d9b2fe",
                "91b68bd8fcf5cf071caf70f9eb0baedd4e64fe10340bcd270292c42cf90263e1",
                "77a107cc14035c6213eb24edba266619d9d5556469702400e151b06342078a77",
                "a6caab6504b9b5211b84e09d86f000d9a76cbbd1c102c7ac1ca16910ea12ba84",
                "dc78465cee2705e0e7df2ab1552b0e397d38b73d7e72748bc15b9bcd79943dc7",
                "348aa2ad2007818206ad5ba02ace1105ff2aac38d651b987f310afad20644fb6"
            ],
            "tx_index": "930",
            "tree_depth": "12",
            "height": "825229",
            "signers": [
                "SP1B0DHZV858RCBC8WG1YN5W9R491MJK88QPPC217",
                "SP3ZCGVGSQWT6TJAXZRXHCYA6SWGNSNGKHSYA1F2Q"
            ],
            "signer_types": [
                "bis",
                "unisat"
            ],
            "signatures": [
                "ef56767f067488b242ba87ccee3c7d4a5e8697d05589d83846ff1bf6ae69d7414703059780e04b1fa5a9cab64d80680a287f9c1d5abab71bc7970785e45d960300",
                "d0341a786a5a7417eded3afca695ac720cf755b59506a6db8ad8a3b553e1b16324707f774cc63a9d8c6da6a3c2c7a1843da1727108ed6bd5106f86791d73750e01"
            ],
            "created_at": 1704970835029,
            "updated_at": 1704970835029
        }
    ]
}
```
{% endtab %}

{% tab title="Latest balance of {tick} by {from_or_to}" %}


**Body**

```
{
    "from_or_to": [
        "bc1p06r44ervnukj3kxnqt863sz9hly5m7f80k7l94aplnd6z2tnrzvstdkzsq"
    ],
    "tick": [
        "ORMM"
    ],
    "limit": 1
}
```

**Response**

```
{
    "data": [
        {
            "tx_hash": "0200000000010269f33e6de0c8d0fa2ad24fb7ab31e62981085c67773072cfebb30ba7e06152230000000000fdffffff14dc94d279bd19377015616ae8a0d7af7debfd4af66a49c4fc84fd1fb7d8d8350100000000fdffffff0322020000000000002251207e875ae46c9f2d28d8d302cfa8c045bfc94df9277dbdf2d7a1fcdba129731899de28080000000000225120b87be26d45435dcf999a5614f87153ba9f12b3ad7d6f7eb89c7096ba55d94a550000000000000000386a360c00000002046465737401000000000000000000000000000000000475736572051676a3e7ab979a2926cac65cf383758c964fd42fad01402130d7dd3c3261b8ffa39397d124b4575e79fae8301fc6240811ed731db4ca3ea1fc92489e0caab3ca70ad4e249cdb3581c39d7cefda14130cd774871c02a6d701408efe9991f8fcc75cf1bcb4b2f905604716abf7f4c6b794a8224955f29dd68fa2952c0b7ac6c2559686241c32b9eaddcaefe146d0ee4e8dea5d1439e0790213b700000000",
            "order_hash": "7ed54d9d2390f2c03ebc479c7b85edf4fcba580f1172073c9102e02b8724a99f",
            "amt": "2696850000000000000000000",
            "bitcoin_tx": "00000024759a2a96f6d4c7b6169e0ec425ce216d6d06ef9e67cb030000000000000000007481d64f1c2113dedb79d8d27b9b3ae829a951aab141428042ec67c5a381bb1accb8a56569d8031718108c2f",
            "decimals": "18",
            "from": {
                "pkscript": "5120b87be26d45435dcf999a5614f87153ba9f12b3ad7d6f7eb89c7096ba55d94a55",
                "address": "bc1phpa7ym29gdwulxv62c20su2nh2039vad04hhawyuwztt54weff2suawxly"
            },
            "from_bal": "0",
            "offset": "0",
            "output": "0",
            "tick": "ORMM",
            "to": {
                "pkscript": "51207e875ae46c9f2d28d8d302cfa8c045bfc94df9277dbdf2d7a1fcdba129731899",
                "address": "bc1p06r44ervnukj3kxnqt863sz9hly5m7f80k7l94aplnd6z2tnrzvstdkzsq"
            },
            "to_bal": "613048439850300100000000000",
            "tx_id": "508358f6ddda009c3384b06485d0ab574673cf099a95e8a842d513d6e0ca9988",
            "proof_hashes": [
                "78d43aeafbf896c9c08ca81dd0123cee8fe6920ec3704ebe4bb1ab379ddc3237",
                "65beee3a9d7539ee0d4bf18ab60e37e0ac4eb8b56eb6a2a6179093d04cbd8bfa",
                "b64a594738212c1a174612558fc88dd25a0e916373c8e516874cc8aa6c7a0ac5",
                "8fa03e9cce09e81847792f1db0a99cad6a24c17879f84186cbd754959f90a6e2",
                "ab41c3f5c9126df65c4a485db493ecf1c7ed6b621469a3bb26143191e47933db",
                "ee5b3305dcdd34d89e87e051e200e8eae6808f0b2002f8ea177578f62983ec76",
                "a6985409cddec722494d07b4a6b699a9095e9ba6fe3b5c902c33bc6213043573",
                "58da9a56a684ac8b76fe1659a0f06bebae67ef17b17352c7cfa3735cf217fc04",
                "dfe3fa38798e03ec6304fd40045345ed99fecb90f6aab0e887cc44691c600ab3",
                "92249cc6082a2cf61718bd52d9f8506ad9bef28d426524455e7ffc1155e99e0d",
                "297e44492b16e57842e2691fbd9c8a1c73677102c81b368b8c10cb538215e686",
                "1d98b08c6ff58476fa9c08d56436a09242ff9c300429f5549303cc89559ee7aa"
            ],
            "tx_index": "2275",
            "tree_depth": "12",
            "height": "825936",
            "signers": [
                "SP1B0DHZV858RCBC8WG1YN5W9R491MJK88QPPC217",
                "SP3ZCGVGSQWT6TJAXZRXHCYA6SWGNSNGKHSYA1F2Q"
            ],
            "signer_types": [
                "bis",
                "unisat"
            ],
            "signatures": [
                "c6a17ad9c249f896dc3a9cc46b77c25d05516811dec70b7768e306a8a79e073932148075c2eb887248534ccfde74da2ac5186f01a847ca00195e35ec2745b98a00",
                "745f23a5993799c6309820b2c2163dbbc5e3f1f8722c643fa58ef016a7d6240540182e5e61320955109b56dec0fd8f51edf25a34686c860ed8d722807c18804601"
            ],
            "created_at": 1705359837844,
            "updated_at": 1705359837844
        }
    ]
}
```
{% endtab %}
{% endtabs %}



