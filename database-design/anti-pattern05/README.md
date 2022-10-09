## 解答

## 課題 1

### 成約・成約破棄を正しく記録できない

以下のような問題が発生し得る。  
ある NewCustomer が成約してくれたが、種々の事情により成約を破棄したいと申し出たとする。このようなケースが発生した際に現状の設計だと、成約前(`NewCustomer.closed=false`), 成約(`NewCustomer.closed=true`), 成約破棄(`NewCustomer.closed=false`)となるため、成約前と成約破棄した NewCustomer の区別が DB のレコードからは不可能である。すなわち、成約というイベントを DB で正確に記録することが難しくなる。

### 面談回数・n 回目の面談日時が記録できない

現状のカラム(`metOnce`, `metAt`)だけだと、面談回数と n 回目の面談日時が記録できない。本来はイベントとして DB 上に記録しておきたいはずだが、現状の設計だと無理。

### NewCustomerテーブルがCallNoteという別の情報を持ってしまっている

`NewCustomer`というテーブル名なのに、電話をかけるというイベントに付随した情報を持っているのがおかしいと思いました。
NewCustomerテーブル内で関心事として持つべき情報では無いと思います。

## 課題 2

![tags (15)](https://user-images.githubusercontent.com/76472239/194740669-b55b048a-6eef-4777-867a-0d110cb785fe.png)

イミュータブルデータモデリングに基づいてテーブル設計を再構築した。
課題１で挙げた問題点は解決したように思われる。

## 課題3

イベントとリソースを適切に抽出せずにモデリングを行なってしまうと陥ってしまう。以下のemployeeテーブルは、所属部署が変わるたびにdepartment_idカラムの値が更新されるため、「どの期間、どの部署に所属していたのか」が記録できない。部署への配属をイベントとして別テーブルに切り出せば良いと考える。

![Employees (1)](https://user-images.githubusercontent.com/76472239/194741079-7057848d-100d-4af5-bc43-dac3f0239c71.png)
