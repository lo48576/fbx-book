# 属性

<!-- TODO: xref -->

本節では FBX バイナリ形式における属性の表現を解説する。

属性のデータ構造についての概要は[『共通の構造』節の『ノード』節](../common-structure/node.md#attributes)を参照。

## 概要<span id="abstract"><!-- --></span>

属性は、以下のように表現される。

| 内容 | サイズ | 補足 |
|------|--------|------|
| [型コード](#value-types) | 1 byte | ASCII アルファベット |
| [型特有の追加ヘッダ](#type-specific-headers) | (型による) | |
| 値 | (型による) | |

## 値の型<span id="value-types"><!-- --></span>

[別の節](../common-structure/node.md#attributes)で既に解説したが再掲すると、属性の値の型は以下のいずれかである。

* [プリミティブ](primitive.md)
    + `bool`
    + `i16`
    + `i32`
    + `i64`
    + `f32`
    + `f64`
* [配列](array.md)
    + `Vec<bool>`
    + `Vec<i32>`
    + `Vec<i64>`
    + `Vec<f32>`
    + `Vec<f64>`
* [特殊](special.md)
    + `Vec<u8>`
    + `String`

## 型コード<span id="type-codes"><!-- --></span>

型コードは属性値の型を表現する1バイトの整数 (`u8`) である。
実際には型コードの値には ASCII のアルファベットのみが利用されている。

以下は型コードの一覧である。

| 型 | 型コード (ASCII) | 型コード (整数) | 命名由来の推測 |
|----|-----------------------|-----------------|----------------|
| `bool` | `C` | `0x43` | Condition の `C`? |
| `i16` | `Y` | `0x59` | 由来不明 |
| `i32` | `I` | `0x49` | Int の `I` |
| `i64` | `L` | `0x4c` | Long の `L` |
| `f32` | `F` | `0x46` | Float の `F` |
| `f64` | `D` | `0x44` | Double の `D` |
| `Vec<bool>` | `b` | `0x62` | bool の `b`? |
| `Vec<i32>` | `i` | `0x69` | int の `i` |
| `Vec<i64>` | `l` | `0x6c` | long の `l` |
| `Vec<f32>` | `f` | `0x66` | float の `f` |
| `Vec<f64>` | `d` | `0x64` | double の `d` |
| `Vec<u8>` | `R` | `0x52` | Raw の `R`? |
| `String` | `S` | `0x53` | String の `S` |

## 型特有の追加ヘッダ<span id="type-specific-headers"><!-- --></span>

型特有の追加ヘッダは、配列型のためのヘッダと特殊型のためのヘッダが存在する。
プリミティブ型に追加のヘッダはない。
