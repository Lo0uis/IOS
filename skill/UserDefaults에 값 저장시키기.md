## userDefaults에 값 저장시키기

userDefaults는 앱에서 일종의 저장소와 같은 역할을 합니다.\
앱 내에서 특정 값을 계속 필요로 할 때, 또는 여러 view Controller에서 한 값을 계속 필요로 할 때 이용하면 좋습니다.

userDefaults는 mutableDictionary와 비슷한 형태로 되어있으며,\
```key```와 ```value```로 값을 저장하고, ```key```값으로 저장되어있는 데이터를 불러옵니다.

이때, 저장하려는 data의 type에 따라 저장하는 방식이 조금 차이가 있습니다.\
```NSString```과 같이 가벼운 값들은 ```mutableDictionary```와 같이 저장하면 되지만,\
```NSArray```나 ```NSDictionary```같이 무거운 애들은 Archaive, 즉 serialize를 해줘야 저장이 가능합니다.\
쉽게말해, 압축한다고 생각하면 됩니다.

밑에서 예시를 통해 보여드리겠습니다.
****
**<data의 Type이 NSString과 같이 가벼울 때, 저장하기>**

```
NSUserDefaults *USER_DEFAULTS = [NSUserDefaults standardUserDefaults];
[USER_DEFAULTS setObject:data forKey:@"sampleKey"] ;
```

**<data의 Type이 NSString과 같이 가벼울 때, 불러오기>**

```
[USER_DEFAULTS objectForKey:@"sampleKey"] ;
```

****

**<data의 Type이 NSArray, NSDictionary와 같이 무거울 때, 저장하기>**

```
NSUserDefaults *USER_DEFAULTS = [NSUserDefaults standardUserDefaults];
NSData *data = [NSKeyedArchiver archivedDataWithRootObject:object];
[USER_DEFAULTS setObject:data forKey:@"sampleKey"];
```

**<data의 Type이 NSArray, NSDictionary와 같이 무거울 때, 불러오기>**

```
NSUserDefaults *USER_DEFAULTS = [NSUserDefaults standardUserDefaults];
NSData *data = [USER_DEFAULTS objectForKey:@"sampleKey"];

[NSKeyedUnarchiver unarchiveObjectWithData:data];
```
