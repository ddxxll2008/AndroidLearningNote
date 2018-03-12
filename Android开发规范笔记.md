##三、Android基本组件

【强制】Activity 间通过隐式 Intent 的跳转，在发出 Intent 之前必须通过 resolveActivity
检查，避免找不到合适的调用组件，造成 ActivityNotFoundException 的异常。

```
public void viewUrl(String url, String mimeType) {
	Intent intent = new Intent(Intent.ACTION_VIEW);
	intent.setDataAndType(Uri.parse(url), mimeType);
	if (getPackageManager().resolveActivity(intent, 	PackageManager.MATCH_DEFAULT_
ONLY) != null) {
		try {
		startActivity(intent);
	} catch (ActivityNotFoundException e) {
	if (Config.LOGD) {
	Log.d(LOGTAG, "activity not found for " + mimeType + " over " +
	Uri.parse(url). getScheme(), e);
			}
		}
	}
}
```

避免在 Service#onStartCommand()/onBind()方法中执行耗时操作，避免在 BroadcastReceiver#onReceive()中执行耗时操作

