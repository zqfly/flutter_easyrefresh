# Code example
~~~dart
class _BasicPageState extends State<BasicPage> {

  List<String> addStr=["1","2","3","4","5","6","7","8","9","0"];
  List<String> str=["1","2","3","4","5","6","7","8","9","0"];
  GlobalKey<EasyRefreshState> _easyRefreshKey = new GlobalKey<EasyRefreshState>();
  GlobalKey<RefreshHeaderState> _headerKey = new GlobalKey<RefreshHeaderState>();
  GlobalKey<RefreshFooterState> _footerKey = new GlobalKey<RefreshFooterState>();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(Translations.of(context).text("basicUse")),
      ),
      body: Center(
          child: new EasyRefresh(
            key: _easyRefreshKey,
            autoLoad: false,
            behavior: ScrollOverBehavior(),
            refreshHeader: ClassicsHeader(
              key: _headerKey,
              refreshText: Translations.of(context).text("pullToRefresh"),
              refreshReadyText: Translations.of(context).text("releaseToRefresh"),
              refreshingText: Translations.of(context).text("refreshing") + "...",
              refreshedText: Translations.of(context).text("refreshed"),
              moreInfo: Translations.of(context).text("updateAt"),
              bgColor: Colors.transparent,
              textColor: Colors.black87,
              moreInfoColor: Colors.black54,
              showMore: true,
            ),
            refreshFooter: ClassicsFooter(
              key: _footerKey,
              loadText: Translations.of(context).text("pushToLoad"),
              loadReadyText: Translations.of(context).text("releaseToLoad"),
              loadingText: Translations.of(context).text("loading"),
              loadedText: Translations.of(context).text("loaded"),
              noMoreText: Translations.of(context).text("noMore"),
              moreInfo: Translations.of(context).text("updateAt"),
              bgColor: Colors.transparent,
              textColor: Colors.black87,
              moreInfoColor: Colors.black54,
              showMore: true,
            ),
            child: new ListView.builder(
              //ListView的Item
                itemCount: str.length,
                itemBuilder: (BuildContext context,int index){
                  return new Container(
                      height: 70.0,
                      child: Card(
                        child: new Center(
                          child: new Text(str[index],style: new TextStyle(fontSize: 18.0),),
                        ),
                      )
                  );
                }
            ),
            onRefresh: () async{
              await new Future.delayed(const Duration(seconds: 1), () {
                setState(() {
                  str.clear();
                  str.addAll(addStr);
                });
              });
            },
            loadMore: () async {
              await new Future.delayed(const Duration(seconds: 1), () {
                if (str.length < 20) {
                  setState(() {
                    str.addAll(addStr);
                  });
                }
              });
            },
          )
      ),
      persistentFooterButtons: <Widget>[
        FlatButton(onPressed: () {
          _easyRefreshKey.currentState.callRefresh();
        }, child: Text(Translations.of(context).text("refresh"), style: TextStyle(color: Colors.black))),
        FlatButton(onPressed: () {
          _easyRefreshKey.currentState.callLoadMore();
        }, child: Text(Translations.of(context).text("loadMore"), style: TextStyle(color: Colors.black)))
      ],// This trailing comma makes auto-formatting nicer for build methods.
    );
  }
}
~~~

# Props Table
| Attribute Name     |     Attribute Explain     | Parameter Type | Default Value  | Requirement |
|---------|--------------------------|:-----:|:-----:|:-----:|
| key | EasyRefresh key     | GlobalKey<EasyRefreshState>  | null | optional(Used for manual trigger loading and refreshing) |
| child      | your content View     | ? extends ScrollView   |   null |  necessary |
| autoLoad | Bottom Autoloading     | bool  | false | optional |
| behavior | Crossing the line effect (with halo or rebound)     | ScrollBehavior | RefreshBehavior | optional |
| refreshHeader | Header view     | RefreshHeader | ClassicsHeader | optional |
| refreshFooter | Footer view     | RefreshFooter | ClassicsFooter | optional |
| onRefresh | Refresh callback method     | () => Void | null | optional(Cannot trigger refresh for null) |
| loadMore | Loading callback method     | () => Void | null | optional(Cannot trigger loading for null) |
| animationStateChangedCallback | EasyRefresh state callback for manual handling of refresh loading and other operations     | (AnimationStates, RefreshBoxDirectionStatus) => void     | null | optional(不推荐使用) |
