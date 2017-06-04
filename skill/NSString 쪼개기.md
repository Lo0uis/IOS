## NSString 쪼개기

**<어떠한 기준으로, 예를 들어 ```\``` or ```.```등으로 NSString을 쪼개고 싶을 때>**

```
NSString *stringUrl = @"www.sample.com/files";
NSArray *arrUrl = [stringUrl componentsSeparatedByString:@"/"];
```

하게되면,
```
arrUrl[0] = @"www.sample.com";
arrUrl[1] = @"files";
```

가 된다.
