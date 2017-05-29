## tableView 최상위로 위치 이동시키기

**<VC간의 뷰 이동을 하면서 테이블 뷰의 위치를 항상 맨위로 하고 싶을 때 사용>**

```
[_tableView setContentOffset:CGPointMake(0.0f, 0.0f)];
```