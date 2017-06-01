## tableView Delegate

테이블 뷰를 사용하게 된다면, 무조건 사용해야 하는 함수들이 있습니다.\
```label```이나 ```button```과 달리, 테이블 뷰는 스스로 할 수 있는 일이 거의 없습니다.\
그래서 우리는 Delegate라는 놈을 사용하여 테이블뷰가 해야할 일을 대신해주게 해야합니다.

**단, Delegate는 그냥 사용할 수는 없고, File's Owner에 상속을 시켜야합니다.\
이 작업은 xib에서 테이블 뷰를 클릭 후, ```Connections Inspector```에서 ```dataSource```와 ```delegate```를 ```File's Owner```에 상속시킵니다.**

그러고나서, 코드로 들어가서 다음과 같은 3가지의 delegate를 선언하면 됩니다.

### <cell의 갯수 delegate>
```
- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {
    return [_tableView.data count] ;
}
```

### <cell의 크기 delegate>
```
- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath {
    
    CellData *cellData = [_tableView.data objectAtIndex:indexPath.row] ;
    
    return cellData.size.height ;
}
```

### <cell클릭 시 delegate>
```
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {
    
    CellData *cellData = [_tableView.data objectAtIndex:indexPath.row] ;
    
    cellSample *cell = [_tableView dequeueReusableCellWithIdentifier:@"cellSample"] ;
    
    [cell bindCellData:cellData] ;
    
    [cell registTouchUpInsideBlock:^(NSInteger tag, CellData *cellData) {
        //테이블 뷰의 셀을 클릭했을 때의 행동을 이곳에서 작성하면 됩니다.
    }] ;
    
    return cell ; 
}
```