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

***

#### UIContextualAction 주의사항

Q. handler의 success 메소드 내부에 해당 액션의 동작이 완료된 후에도 해당 row 의 Swipe 가 계속 활성화 되는 문제가 있음   
A. success(true) 와 같이 호출해줌으로써 완료를 의미하는 코드를 작성해야 함.
```
let deleteAction: UIContextualAction
deleteAction = UIContextualAction(style: UIContextualAction.Style.destructive,
                                  title: "delete",
                                  handler: { (UIContextualAction, UIView, success: @escaping (Bool) -> Void) in
                                    print("exec delete")
                                    
                                    success(true)
                                  })
deleteAction.backgroundColor = .systemRed
deleteAction.image = UIImage(systemName: "trash.fill")
```
> Escaping Closure 관련 개념을 숙지해야 올바르게 이해가 가능할 것으로 보인다.

***

#### TableView reload 경고 에러

Q. 두번째_컨트롤러 에서 첫번째_컨트롤러 의 TableView 를 reload 를 할 때 경고가 발생한다.   
> 2021-09-26 15:34:55.578327+0900   
> Reminders[1991:9837716] [TableView] Warning once only: UITableView was told to layout its visible cells and other contents without being in the view hierarchy (the table view or one of its superviews has not been added to a window). This may cause bugs by forcing views inside the table view to load and perform layout without accurate information (e.g. table view bounds, trait collection, layout margins, safe area insets, etc), and will also cause unnecessary performance overhead due to extra layout passes. Make a symbolic breakpoint at UITableViewAlertForLayoutOutsideViewHierarchy to catch this in the debugger and see what caused this to occur, so you can avoid this action altogether if possible, or defer it until the table view has been added to a window. Table view: <UITableView: 0x7fdded035000; frame = (0 0; 414 896); clipsToBounds = YES; autoresize = W+H; gestureRecognizers = <NSArray: 0x60000043a820>; layer = <CALayer: 0x600000a38560>; contentOffset: {0, -92}; contentSize: {414, 131.5}; adjustedContentInset: {92, 0, 83, 0}; dataSource: <Reminders.ListTableViewController: 0x7fddee90ac30>>   

A.   
* 첫번째_컨트롤러에서 Notification 을 받아 reload를 하도록 처리하는 부분이 오류로 생각해서 viewWillAppear() 로 reload 코드를 옮겼다.   
(진작에 이렇게 하는게 맞는데 굳이 Notification 을 썼지..)
* 그러나 계속 같은 오류가 발생해서 백그라운드에서 처리하기 위해 DispatchQueue.main.async() 에서 처리하도록 변경했더니 경고 에러가 처리됐다. (View 조작은 메인쓰레드에서!)
* 대강 이유는 이미 View 가 다 그려졌는데 그 후에 View 를 변경하는 행위는 iOS에서 에러로 보려고 한다. 추후 문제가 발생할 소지가 있어보여 경고를 한 것이다.   

참고: https://stackoverflow.com/questions/57897288/uitableviewalertforlayoutoutsideviewhierarchy-error-warning-once-only-ios-13-g

