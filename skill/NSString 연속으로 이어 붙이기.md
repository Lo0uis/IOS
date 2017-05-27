## NSString 연속으로 이어 붙이기

**<2개 이상의 string을 연속으로 붙이고 싶을 때 쓰면 됩니다.>**

```
NSString *newString = [NSString stringWithFormat:@"%@%@",sample,smaple2];
or
NSString *newString = [string1 appendString:string2];
```
