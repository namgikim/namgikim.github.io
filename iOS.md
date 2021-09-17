## iOS

### 폰트 에러

Q. 에러 메시지
> 2021-09-16 09:21:29.049308+0900 TodosRe[75049:7155229] CoreText note: Client requested name ".AppleSDGothicNeoI-Regular", it will get TimesNewRomanPSMT rather than the intended font. All system UI font access should be through proper APIs such as CTFontCreateUIFontForLanguage() or +[UIFont systemFontOfSize:].   
> 2021-09-16 09:21:29.049539+0900 TodosRe[75049:7155229] CoreText note: Set a breakpoint on CTFontLogSystemFontNameRequest to debug.

A.
AppleSDGothicNeo 관련 폰트 에러라고 한다.   
TextView 의 Text 속성에 글을 미리 작성했더니 위와 같이 에러가 발생했다.   
이 속성은 비워두고, 코드로 채워서 처리했다.   

***

#### TableView.reloadSections(_:with:) 에러

Q. 에러 메시지
> 2021-09-17 14:43:04.834539+0900 TodoSimple[76502:7349785] [TableView] Warning once only: UITableView was told to layout its visible cells and other contents without being in the view hierarchy (the table view or one of its superviews has not been added to a window). This may cause bugs by forcing views inside the table view to load and perform layout without accurate information (e.g. table view bounds, trait collection, layout margins, safe area insets, etc), and will also cause unnecessary performance overhead due to extra layout passes. Make a symbolic breakpoint at UITableViewAlertForLayoutOutsideViewHierarchy to catch this in the debugger and see what caused this to occur, so you can avoid this action altogether if possible, or defer it until the table view has been added to a window. Table view: <UITableView: 0x7f8607846600; frame = (0 0; 414 896); clipsToBounds = YES; autoresize = W+H; gestureRecognizers = <NSArray: 0x6000021e3090>; layer = <CALayer: 0x600002fc5ea0>; contentOffset: {0, -92}; contentSize: {414, 156}; adjustedContentInset: {92, 0, 34, 0}; dataSource: <TodoSimple.TodosTableViewController: 0x7f8606e13200>>

A.
TableViewController 의 viewDidAppear() 가 아닌 viewWillAppear() 에 작성하여 위와 같은 에러가 발생함.   
동작에 문제는 없어보였지만, viewDidAppear() 로 수정하여 처리함.   

