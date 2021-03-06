# Ref

## Scalar Value Types

https://developers.google.com/protocol-buffers/docs/proto3#scalar

## Enumerations

https://developers.google.com/protocol-buffers/docs/proto3#enum

## Packed Repeated Fields

https://developers.google.com/protocol-buffers/docs/encoding#packed

# Description

## ■タグの割り当て

ご覧のとおり、メッセージ定義の各フィールドには、固有の番号付きタグがあります。

これらのタグは、メッセージのバイナリ形式でフィールドを識別するために使用され、メッセージタイプが使用されると変更されるべきではありません。

1〜15の範囲の値を持つタグは、識別番号とフィールドのタイプを含む1バイトのエンコードになります

（これについては、プロトコルバッファエンコーディングで詳しく知ることができます）。

16〜2047の範囲のタグは2バイトとなります。

したがって、非常に頻繁に発生するメッセージ要素に対してタグ1〜15を予約する必要があります。

将来追加される可能性のある頻繁に発生する要素のための余地を残すことを忘れないでください。

## ■フィールド・ルールの指定

メッセージ・フィールドは、次のいずれかになります。

単数形：整形式メッセージは、このフィールドのゼロまたは1つを持つことができます（ただし、複数はありません）。

repeated：このフィールドは、整形式のメッセージで任意の回数（ゼロを含む）繰り返すことができます。繰り返し値の順序は保持されます。

proto3では、スカラー数値型の繰り返しフィールドは、デフォルトでパックされたエンコーディングを使用します。

## ■予約フィールド

フィールドを完全に削除またはコメントアウトしてメッセージタイプを更新した場合、将来のユーザはタイプを更新するときにタグ番号を再利用できます。

これにより、後でデータ破損、プライバシーバグなどを含む同じ.protoの古いバージョンを読み込むと、深刻な問題が発生する可能性があります。

これが起こらないようにする方法の1つは、削除されたフィールドのフィールドタグ（およびJSONシリアライゼーションの問題を引き起こす可能性のある名前）が予約されていることを指定することです。

プロトコルバッファコンパイラは、将来のユーザがこれらのフィールド識別子を使用しようとすると文句を言うでしょう。

## ■デフォルト値

メッセージが解析されるとき、エンコードされたメッセージに特定の特異要素が含まれていない場合、解析されたオブジェクトの対応するフィールドは、そのフィールドのデフォルト値に設定されます。

これらのデフォルトは型固有です。

- 文字列の場合、デフォルト値は空文字列です。
- バイトの場合、デフォルト値は空のバイトです。
- boolの場合、デフォルト値はfalseです。
- 数値型の場合、デフォルト値はゼロです。
- 列挙型の場合、デフォルト値は最初に定義されたenum値です。これは0でなければなりません。
- メッセージフィールドの場合、フィールドは設定されません。その正確な値は言語に依存します。詳細については、生成されたコードガイドを参照してください。

繰り返しフィールドのデフォルト値は空です（通常、適切な言語の空のリスト）。

スカラー・メッセージ・フィールドの場合、メッセージが解析されると、フィールドが明示的にデフォルト値に設定されているかどうか（例えばブール値がfalseに設定されているかどうかなど）これは、あなたのメッセージタイプを定義する際に留意してください。

たとえば、デフォルトでその動作を行わないようにするには、falseに設定されているときに何らかの動作を切り替えるブール値を持たないでください。

また、スカラーメッセージフィールドがデフォルトに設定されている場合、値はワイヤでシリアル化されません。

生成されたコードでのデフォルトの動作の詳細については、選択した言語の生成コードガイドを参照してください。

https://developers.google.com/protocol-buffers/docs/reference/overview

## ■列挙型

メッセージ型を定義するとき、そのフィールドの1つに値の事前定義済みリストの1つだけを入れることができます。

たとえば、コーパスがUNIVERSAL、WEB、IMAGES、LOCAL、NEWS、PRODUCTS、またはVIDEOのいずれかのコーデックフィールドを各SearchRequestに追加したいとします。

可能な値ごとに定数を指定してメッセージ定義に列挙型を追加することで、これを非常に簡単に行うことができます。

ご覧のように、コーパス列挙体の最初の定数はゼロにマップされます。

すべての列挙型定義には、最初の要素としてゼロにマップされる定数が含まれている必要があります。

これは、以下の理由によるものです。

- 数字のデフォルト値として0を使用できるように、ゼロ値が必要です。

- 最初の列挙型の値が常にデフォルトであるproto2セマンティクスとの互換性のために、0の値が最初の要素である必要があります。

別の列挙定数に同じ値を代入することによって、別名を定義することができます。

これを行うには、allow_aliasオプションをtrueに設定する必要があります。

そうしないと、エイリアスが見つかったときにプロトコルコンパイラによってエラーメッセージが生成されます。

列挙定数は32ビット整数の範囲内になければなりません。

列挙型の値はワイヤでバリント符号化を使用するため、負の値は効率が悪く、推奨されません。

上記の例のように、メッセージ定義内でenumを定義することができます。

これらのenumは、.protoファイル内の任意のメッセージ定義で再利用できます。

また、MessageType.EnumTypeという構文を使用して、あるメッセージで宣言された列挙型を別のメッセージのフィールドの型として使用することもできます。

列挙型を使用する.protoでプロトコルバッファコンパイラを実行すると、生成されたコードにはJavaまたはC ++の対応する列挙型があります。

実行時に整数値を持つシンボル定数のセットを作成するために使用されるPython用の特別なEnumDescriptorクラス生成されたクラス。

デシリアライズ時には、メッセージに認識されない列挙型の値が保持されますが、メッセージがデシリアライズされたときの表現方法は言語に依存します。

C ++やGoなど、指定されたシンボルの範囲外の値を持つオープンな列挙型をサポートする言語では、未知の列挙型の値は単純にその基礎となる整数表現として格納されます。

Javaなどの閉じた列挙型を持つ言語では、列挙型の大文字小文字が認識されない値を表すために使用され、基底の整数は特別なアクセサでアクセスできます。

どちらの場合でも、メッセージがシリアライズされても、認識されない値はメッセージと共にシリアル化されます。

## ■デフォルト値

メッセージが解析されるとき、エンコードされたメッセージに特定の特異要素が含まれていない場合、解析されたオブジェクトの対応するフィールドは、そのフィールドのデフォルト値に設定されます。

これらのデフォルトは型固有です。

- 文字列の場合、デフォルト値は空文字列です。
- バイトの場合、デフォルト値は空のバイトです。
- boolの場合、デフォルト値はfalseです。
- 数値型の場合、デフォルト値はゼロです。
- 列挙型の場合、デフォルト値は最初に定義されたenum値です。これは0でなければなりません。
- メッセージフィールドの場合、フィールドは設定されません。

その正確な値は言語に依存します。詳細については、生成されたコードガイドを参照してください。

繰り返しフィールドのデフォルト値は空です（通常、適切な言語の空のリスト）。

スカラー・メッセージ・フィールドの場合、メッセージが解析されると、フィールドが明示的にデフォルト値に設定されているかどうか（たとえば、ブール値がfalseに設定されているかどうかなど）、あるいはまったく設定されていないかどうかを示す方法はありません

あなたのメッセージタイプを定義する際に気をつけてください。

たとえば、デフォルトでその動作を行わないようにするには、falseに設定されているときに何らかの動作を切り替えるブール値を持たないでください。

また、スカラーメッセージフィールドがデフォルトに設定されている場合、値はワイヤでシリアル化されません。

生成されたコードでのデフォルトの動作の詳細については、選択した言語の生成コードガイドを参照してください。

## ■列挙型

メッセージ型を定義するとき、そのフィールドの1つに値の事前定義済みリストの1つだけを入れることができます。

たとえば、コーパスがUNIVERSAL、WEB、IMAGES、LOCAL、NEWS、PRODUCTSまたはVIDEOのコーデックフィールドを各SearchRequestに追加するとします。

可能な値ごとに定数を指定してメッセージ定義に列挙型を追加することで、これを非常に簡単に行うことができます。

ご覧のように、コーパス列挙体の最初の定数はゼロにマップされます。

すべての列挙型定義には、最初の要素としてゼロにマップされる定数が含まれている必要があります。

これは、以下の理由によるものです。

- 数字のデフォルト値として0を使用できるように、ゼロ値が必要です。

- 最初の列挙型の値が常にデフォルトであるproto2セマンティクスとの互換性のために、0の値が最初の要素である必要があります。

別の列挙定数に同じ値を代入することによって、別名を定義することができます。

これを行うには、allow_aliasオプションをtrueに設定する必要があります。

そうしないと、エイリアスが見つかったときにプロトコルコンパイラによってエラーメッセージが生成されます。

列挙定数は32ビット整数の範囲内になければなりません。

列挙型の値はワイヤー上でヴァリント符号化を使用するため、負の値は非効率的であり推奨されません。

上記の例のように、メッセージ定義内でenumを定義することができます。

これらのenumは、.protoファイル内の任意のメッセージ定義で再利用できます。

また、MessageType.EnumTypeという構文を使用して、あるメッセージで宣言された列挙型を別のメッセージのフィールドの型として使用することもできます。

enumを使用する.protoでプロトコルバッファコンパイラを実行すると、生成されたコードは、JavaまたはC ++の対応する列挙型を持ちます。

これは、整数値を持つシンボル定数のセットを作成するために使用されるPython用の特別なEnumDescriptorクラスです。

ランタイム生成クラス。デシリアライズ時には、メッセージに認識されない列挙値が保持されますが、メッセージのデシリアライズ時の表現方法は言語に依存します。

C ++やGoなど、指定されたシンボルの範囲外の値を持つオープンな列挙型をサポートする言語では、未知の列挙型の値は単純にその基礎となる整数表現として格納されます。

Javaなどの閉じた列挙型を持つ言語では、列挙型の大文字小文字は認識されない値を表すために使用され、基底の整数は特別なアクセサでアクセスできます。

どちらの場合でも、メッセージがシリアライズされても、認識されない値はメッセージと共にシリアル化されます。

アプリケーションでメッセージ列挙を処理する方法の詳細については、選択した言語の生成コードガイドを参照してください。

## ■予約済み値

列挙型を完全に削除したり、コメントアウトして列挙型を更新した場合、将来のユーザーはその型を独自に更新するときに数値を再利用できます。

これにより、後でデータ破損、プライバシーバグなどを含む同じ.protoの古いバージョンを読み込むと、深刻な問題が発生する可能性があります。

これが起こらないようにする方法の1つは、削除されたエントリの数値（JSONシリアライゼーションの問題を引き起こす可能性のある名前）が予約されていることを指定することです。

プロトコルバッファコンパイラは、将来のユーザがこれらの識別子を使用しようとすると文句を言うでしょう。

予約された数値の範囲は、maxキーワードを使用して最大可能な値まで上がるように指定できます。

同じ予約済みの文でフィールド名と数値を混在させることはできません。

## ■他のメッセージタイプの使用

フィールドタイプとして他のメッセージタイプを使用できます。

たとえば、各SearchResponseメッセージにResultメッセージを含める場合は、同じ.protoでResultメッセージタイプを定義し、SearchResponseにResult型のフィールドを指定します。

## ■定義のインポート

上記の例では、結果メッセージタイプはSearchResponseと同じファイルで定義されています。

フィールドタイプとして使用するメッセージタイプが別の.protoファイルですでに定義されている場合はどうなりますか？

他の.protoファイルの定義をインポートして使用できます。別の.protoの定義をインポートするには、ファイルの先頭にimport文を追加します。

デフォルトでは、直接インポートされた.protoファイルの定義のみを使用できます。

ただし、.protoファイルを新しい場所に移動する必要が生じることがあります。

.protoファイルを直接移動し、すべての呼び出しサイトを1回の変更で更新する代わりに、ダミーの.protoファイルを古い場所に配置して、インポートの概念を使用してすべてのインポートを新しい場所に転送することができます。

インポート・パブリック依存関係は、インポート・パブリック・ステートメントを含むprotoをインポートするすべての人に過渡的に依存することができます。

## ■ネストされたタイプ

次の例のように、他のメッセージタイプ内でメッセージタイプを定義して使用することができます。

ここでは、ResultResponseメッセージ内にResultメッセージが定義されています。

```
message SearchResponse {
  message Result {
    string url = 1;
    string title = 2;
    repeated string snippets = 3;
  }
  repeated Result results = 1;
}
```

このメッセージタイプを親メッセージタイプの外で再利用する場合は、Parent.Typeとして参照します。

```
message SomeOtherMessage {
  SearchResponse.Result result = 1;
}
```

メッセージを好きなだけ深く入れ子にすることができます：

## ■メッセージタイプを更新する

既存のメッセージタイプがあなたのニーズをすべて満たしていない場合（たとえば、メッセージフォーマットに余分なフィールドを追加したい場合など）、古いフォーマットで作成したコードを使用したい場合は、心配しないで！既存のコードを壊さずにメッセージタイプを更新するのは非常に簡単です。

次のルールを覚えておいてください：

see https://developers.google.com/protocol-buffers/docs/proto3#updating

## ■未知フィールド
未知フィールドは、正常に形成されたプロトコルバッファであり、パーサーが認識できないフィールドを表す直列化データです。

たとえば、古いバイナリが新しいバイナリによって新しいフィールドで送信されたデータを解析する場合、それらの新しいフィールドは古いバイナリの未知のフィールドになります。

Proto3の実装は、未知のフィールドを持つメッセージを首尾よく解析できますが、実装は未知のフィールドを保持することをサポートしている場合とサポートしていない場合があります。

未知のフィールドが保持されたり、削除されたりすることに頼るべきではありません。

ほとんどのGoogleプロトコルバッファ実装では、未知のフィールドはproto3では対応するプロトタイプのランタイムを介してアクセスできず、脱直列化時に削除され、忘れてしまいます。

これはproto2とは異なる動作で、未知のフィールドは常にメッセージとともに保存され、シリアライズされます。

## ■Any

Anyメッセージタイプを使用すると、.proto定義を持たないメッセージを埋め込みタイプとして使用できます。

Anyには、任意のシリアライズされたメッセージがバイトとして格納されます。

URLは、そのメッセージの種類に対してグローバルに一意な識別子として機能します。

Any型を使用するには、google / protobuf / any.protoをインポートする必要があります。

```
import "google/protobuf/any.proto";

message ErrorStatus {
  string message = 1;
  repeated google.protobuf.Any details = 2;
}
```

指定されたメッセージタイプのデフォルトのタイプURLは、type.googleapis.com/packagename.messagenameです。

JavaでAny型は特別なpack（）とunpack（）アクセサを持ちますが、C ++ではPackFrom（）がありますが、 UnpackTo（）メソッド：

```
// Storing an arbitrary message type in Any.
NetworkErrorDetails details = ...;
ErrorStatus status;
status.add_details()->PackFrom(details);

// Reading an arbitrary message from Any.
ErrorStatus status = ...;
for (const Any& detail : status.details()) {
  if (detail.Is<NetworkErrorDetails>()) {
    NetworkErrorDetails network_error;
    detail.UnpackTo(&network_error);
    ... processing network_error ...
  }
}
```

 現在、Any型で動作するためのランタイムライブラリは開発中です。

すでにproto2の構文に精通しているならば、Any型は拡張を置き換えます。

## ■Oneof

FIXME
