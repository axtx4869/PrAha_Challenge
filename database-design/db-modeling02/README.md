## 回答

テーブル間の関係性については添付画像の通りなので、以下に考えたことなどについてまとめます。

### 1.スレッドにチャンネル ID を持たせるかどうか

スレッドにはチャンネル ID を持たせなくても、親メッセージから算出できるかと思いますが、スレッドを含めたメッセージの検索機能などでチャンネル ID は頻繁に参照される値だと考え、スレッドのテーブルにチェンネル ID を持たせています。

### 2.親メッセージにスレッド有無判定フラグを持たせるかどうか

slack などのタイムライン上でスレッド持っている場合の UI を考えた時に、スレッドを持っている場合と持っていない場合とで違いがあります。したがってフロントエンドにスレッドの有無の情報を返す必要があると考えられますが、スレッド有無の判定の際にスレッドのテーブルから親のメッセージ ID を持つレコードを探索するのは負荷であり、スレッド有無は頻繁に参照される情報だと考えたので、毎回その探索をさせるのは得策では無いと考えました。ゆえに親メッセージにスレッド有無判定フラグを持たせました。  
スレッド有無判定はスレッド数カラム(int)の値が 1 以上か否かでも判定できると思いますが、そうしてしまうと、スレッドを投稿する際に、スレッドの CREATE と親メッセージの UPDATE の二つのクエリが毎回実行されます。bool にすれば実行されるクエリの数を 1 回で済ませられると考えたので bool にしました。(スレッドの CREATE、DELETE の実行の時に場合によってはクエリが 2 回実行されますが、毎回ではないのでベターかなと思いました。)