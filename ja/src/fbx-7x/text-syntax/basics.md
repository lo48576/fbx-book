# 基本

以下は大雑把な文法を ABNF で表現したものである。
これらが正確とは限らない (むしろ、おそらく誤りがある) ことに留意せよ。

```text
ALPHA = %41-5A / %61-7A   ; A-Z / a-z
DIGIT = %x30-39           ; 0-9
ALNUM = ALPHA / DIGIT     ; alphanumeric
HEXDIG = DIGIT / "A" / "B" / "C" / "D" / "E" / "F"
                          ; hex digit
DQUOTE = %x22             ; " (double quote)
HTAB = %x09               ; horizontal tab
LF = %x0A                 ; line feed
WSP = SP / HTAB           ; white space

file = *(comment / node / LF)
comment = *WSP ";" *(ALNUM / WSP) LF
node = *WSP name ": " [attributes] " {" LF children *WSP "}" LF
     / *WSP name ": " attributes LF
name = ALPHA *ALNUM
attributes = attr-value *("," [WSP] attr-value)
attr-value = attr-boolean / attr-number / attr-array / attr-string / attr-binary
attr-boolean = "T" / "Y"
integer = 0 / %x31-39 *DIGIT
attr-number = ["-"] integer ["." integer / "E" ["-"] integer]
attr-array = "*" integer " {" LF *WSP "a: " [attr-array-elem *("," attr-array-elem)] [","] LF *WSP "}"
attr-array-elem = attr-boolean / attr-number
string-content-char = %x00-21 / %x23-25 / "&" ("cr" / "lf" / "quot") ";" / #x27-FF
attr-string = DQUOTE *string-content-char DQUOTE
base64-char = ALNUM / "+" / "/"
base64-chunk = *(4*base64-char)
base64-chunk-final = base64-chunk [
    base64-char "==="
    / 2*base64-char "=="
    / 3*base64-char "="
    / 4*base64-char
    ]
attr-binary = *(DQUOTE base64-chunk DQUOTE "," LF SP) DQUOTE base64-chunk-final DQUOTE [","]
    ; Note that this may have trailing comma.
children = *comment node *(node / comment)
```

## ファイル

ファイルはノードとコメントを並べたものである。
コメントはおそらく任意だが、 FBX SDK が出力したファイルの先頭には以下のようなコメントが付いている。

FBX 7.3 の場合:

```text
; FBX 7.3.0 project file
; Copyright (C) 1997-2010 Autodesk Inc. and/or its licensors.
; All rights reserved.
; ----------------------------------------------------
```

FBX 7.5 の場合:

```text
; FBX 7.5.0 project file
; Copyright (C) 1997-2010 Autodesk Inc. and/or its licensors.
; All rights reserved.
; ----------------------------------------------------
```

これが信頼性のあるファイル形式判別に利用できるかは不明。

## コメント

`;` は行コメント開始記号であり、行末までが無視される。
文字列値中に出現した `;` は、コメント開始記号ではなく単なる文字列の一部として解釈される。
