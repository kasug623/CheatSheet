# Displaying Weekdays on the Dashboard
This is the only way.  
```
emit(doc['order_date'].value.withZoneSameInstant(ZoneId.of('Asia/Tokyo'))
  .dayOfWeek.getDisplayName(TextStyle.SHORT, Locale.JAPAN))
```
