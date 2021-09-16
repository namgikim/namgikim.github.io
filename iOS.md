# iOS

## 폰트 에러

Q.   
2021-09-16 09:21:29.049308+0900 TodosRe[75049:7155229] CoreText note: Client requested name ".AppleSDGothicNeoI-Regular", it will get TimesNewRomanPSMT rather than the intended font. All system UI font access should be through proper APIs such as CTFontCreateUIFontForLanguage() or +[UIFont systemFontOfSize:].
2021-09-16 09:21:29.049539+0900 TodosRe[75049:7155229] CoreText note: Set a breakpoint on CTFontLogSystemFontNameRequest to debug.

A.   
AppleSDGothicNeo 관련 폰트 에러라고 한다.
TextView 의 Text 속성에 글을 미리 작성했더니 위와 같이 에러가 발생했다.
이 속성은 비워두고, 코드로 채워서 처리했다.
