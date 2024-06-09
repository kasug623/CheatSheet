#  ダッシュボード上で曜日を表示したい場合は、ダッシュボード側でtimestampフィールドから曜日を表現する方法はないでしょうか。
# （Elasticsearchにデータを取り込む際にday_of_weekやday_of_week_iといった曜日を表すフィールドを個別に持たせる必要があるのでしょうか？）
ep. KibanaAnalysisで更新に質問した内容
```
emit(doc['order_date'].value.withZoneSameInstant(ZoneId.of('Asia/Tokyo'))
  .dayOfWeek.getDisplayName(TextStyle.SHORT, Locale.JAPAN))
```
