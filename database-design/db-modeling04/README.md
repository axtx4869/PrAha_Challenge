# 解答

![penpen (1)](https://user-images.githubusercontent.com/76472239/189515428-9df6d485-813b-4559-95f3-9f5d39ec70c7.png)

### reminders テーブル

**start_datetime**

初回のリマインド実行日時。"X 日おき"などの基準として必要

**frequency_type**  
`"DAILY", "WEEKLY", "MONTHLY"`のいづれか

**frequency_interval**  
`frequency_type` が  
`DAILY`: 0 ~ 6  
`WEEKLY`: 1 ~ 7(1:月, 2:火,...7:日)  
`MONTHLY` : 1 ~ 31

例:

`frequency_type="DAILY"`で`frequency_interval=0`
→ 毎日リマインド

`frequency_type="DAILY"`で`frequency_interval=3`
→3 日おきにリマインド

`frequency_type="WEEKLY"`で`frequency_interval=1`
→ 毎週月曜日にリマインド

`frequency_type="MONTHLY"`で`frequency_interval=20`
→ 毎月 20 日にリマインド

### batch_logs テーブル

1h おきにバッチが動くので、一日に同一リマインダーが複数回送信されないようにするために`finished_datetime`カラム(バッチの終了時刻)を用意した。

### reminder_recipients テーブル

リマインダーとメンションする対象の関係性を表現するためのテーブル。
