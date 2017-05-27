## NSString 연속으로 이어 붙이기

**<2개 이상의 string을 연속으로 붙이고 싶을 때, %@와 뒤에 변수명만 추가로 해서 쓰면 됩니다.>**

```
NSString *newString = [NSString stringWithFormat:@"%@%@",sample,smaple2];
or
NSString *newString = [string1 appendString:string2];
```

**(objective-c는 참조(*)로 선언하는 모든 타입을 %@로 쓸 수 있습니다. ```NSString```, ```NSArray```, ```NSDictionary``` 등등)**