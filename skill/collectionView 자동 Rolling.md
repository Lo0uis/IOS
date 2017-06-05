## collectionView 자동 Rolling

collectionView에서 광고노출과 같이 자동으로 다음 광고를 rolling해주는 기능을 구현하기 위해서,
기본적인 collectionView delegate 4개외에 추가로 몇개를 더 선언을 해주어야 합니다.

또한 timer를 생성해주어, 지정한 시간마다 rolling되게 할 수 있습니다.

가장 먼저, ```interface```에 몇가지 변수들을 선언합니다.

```
@interface VCFavorites ()
...
@property (strong, nonatomic) IBOutlet UICollectionView *collectionView;

@property (strong, nonatomic) NSTimer *timer ;
@property (nonatomic) NSInteger index ;
@property (nonatomic) NSInteger bannerCount ;
...
@end
```

그다음, 우리가 잘 아는 delegate를 선언해주면 됩니다.

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

### < cell Bind delegate >
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

### <cell 각각의 index delegate>
```
- (NSInteger)collectionViewSelectedIndex:(UIScrollView *)scrollView {
    
    CGFloat pageWidth = scrollView.frame.size.width;
    float currentPage = scrollView.contentOffset.x / pageWidth + 200;
    
    NSInteger index = 0 ;
    
    if (0.0f != fmodf(currentPage, 1.0f)) {
        index = currentPage + 1;
    }
    else {
        index = currentPage + 0;
    }
    
    return index ;
}
```

### <cell scroll delegate>
```
- (void)collectionViewScrollToIndex:(NSInteger) index animated:(BOOL) animated {
    
    [_collectionView scrollToItemAtIndexPath:[NSIndexPath indexPathForRow:index inSection:0] atScrollPosition:UICollectionViewScrollPositionCenteredHorizontally animated:animated] ;
    
}
```

### <cell 무한루프 delegate>
```
- (void)scrollViewDidEndDecelerating:(UIScrollView *)scrollView {
    
    NSInteger page = [self collectionViewSelectedIndex:scrollView] ;
    
    //무한루프
    if (scrollView == _collectionView) {
        
        float contentOffsetWhenFullyScrolledRight = _collectionView.frame.size.width * ([_collectionView.data count] -1);
        
        if (scrollView.contentOffset.x == contentOffsetWhenFullyScrolledRight) {
            
            NSIndexPath *newIndexPath = [NSIndexPath indexPathForItem:1 inSection:0];
            
            [_collectionView scrollToItemAtIndexPath:newIndexPath atScrollPosition:UICollectionViewScrollPositionLeft animated:NO];
            
            
        } else if (scrollView.contentOffset.x == 0)  {
            
            NSIndexPath *newIndexPath = [NSIndexPath indexPathForItem:([_collectionView.data count] -2) inSection:0];
            
            [_collectionView scrollToItemAtIndexPath:newIndexPath atScrollPosition:UICollectionViewScrollPositionLeft animated:NO];
            
        }
    }
}
```

****

### <시작 timer> : 3초마다 rolling, 바인딩이 시작될 때 같이 시작.
```
-(void)startTimer {

    _index = (int)(10000/2)*_bannerCount ; //10000은 임의의 큰 정수
    
    NSIndexPath *newIndexPath = [NSIndexPath indexPathForItem:_index inSection:0];

    [_collectionView scrollToItemAtIndexPath:newIndexPath atScrollPosition:UICollectionViewScrollPositionLeft animated:NO];

    _timer = [NSTimer scheduledTimerWithTimeInterval:3
                                              target:self
                                            selector:@selector(updateTimer:)
                                            userInfo:nil
                                             repeats:YES] ;
    
}
```

### <정지 timer> : viewWillDisappear()에서 호출하면 됨.
```
-(void)stopTImer {
    
    [_timer invalidate] ;
    _timer = nil ;
    
}
```

### <업데이트 timer>
```
- (void)updateTimer:(NSTimer*) timer {
    
    NSIndexPath *newIndexPath = [NSIndexPath indexPathForItem:++_index inSection:0];
    
    if(_index != [_collectionView.data count]) {
        [_collectionView scrollToItemAtIndexPath:newIndexPath atScrollPosition:UICollectionViewScrollPositionLeft animated:YES];
        
    }
    else if(_index == [_collectionView.data count]){
        
        _index = 0 ;
        
        NSIndexPath *newIndexPath = [NSIndexPath indexPathForItem:0 inSection:0];
        
        [_collectionView scrollToItemAtIndexPath:newIndexPath atScrollPosition:UICollectionViewScrollPositionLeft animated:NO];
           
    } 
}
```