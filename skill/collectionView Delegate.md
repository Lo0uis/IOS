## collectionView Delegate

Delegate에 대한 설명은 ```tableView Delegate.md```에서 설명하였으므로, 생략하겠습니다.

collectionView는 최소 4가지의 delegate를 선언해야 합니다.

**(Tip. ```CollectionView```, ```TableView```는 ```ScrollView```를 상속합니다. 즉, ```ScrollView```의 ```Delegate```를 써도 됩니다.)**

### <cell의 갯수 delegate>
```
- (NSInteger)collectionView:(UICollectionView *)collectionView numberOfItemsInSection:(NSInteger)section {
    
    return [collectionView.data count];
}
```

### <cell의 크기 delegate>
```
- (CGSize)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout *)collectionViewLayout sizeForItemAtIndexPath:(NSIndexPath *)indexPath {
    
    CellData *cellData = [collectionView.data objectAtIndex:indexPath.row] ;
    
    return cellData.size ;
}
```

### <cell 클릭 시 delegate>
```
- (void)collectionView:(UICollectionView *)collectionView didSelectItemAtIndexPath:(NSIndexPath *)indexPath {
    
    CellData *cellData = [collectionView.data objectAtIndex:indexPath.row] ;
    
    if (EQUALS(cellData.identifier, @"cellSample")) {
        
        //collectionView의 셀을 클릭했을 때의 행동을 이곳에서 작성하면 됩니다.
            
        }
    }
}
```

### <cell Bind delegate>
```
- (UICollectionViewCell *)collectionView:(UICollectionView *)collectionView cellForItemAtIndexPath:(NSIndexPath *)indexPath {
    
    CellData *cellData = [collectionView.data objectAtIndex:indexPath.row] ;
    
    if (EQUALS(cellData.identifier, @"cellSample")) {
        
        cellSample *cell = [collectionView
                                dequeueReusableCellWithReuseIdentifier:@"cellSample"
                                forIndexPath:indexPath] ;
        
        [cell bindCellData:cellData] ;
        
        return cell;
    }
    
    return nil;
}
```
