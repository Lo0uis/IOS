## 다른 View Controller(VC)에 값 보낼 때

**<바로 이전의 VC에 값을 보내고 싶을때>**

```
VCSample *vc = [SLIDE_NAVIGATION.viewControllers lastObject];
or
VCSample *vc = [SLIDE_NAVIGATION.viewControllers objectAtIndex:SLIDE_NAVIGATION.viewControllers.count -2];
```
**<만약 slideNavigation이 없는 프로젝트에서는>**

```
VCSample *vc = [self.navigationController.viewControllers objectAtIndex:(self.navigationController.viewControllers.count -2)];
```

**하고나서, VCSample에 Public으로 저장되어있는 변수에 값을 저장하면 된다.**

```
vc.변수 = 저장할 값;
```

****

## 다른 곳에서 VC값 받을 때

**<내가 현재 위치가 View나 Cell 등 VC가 아닐때>**

```
VCSample *vc = self.viewCountroller;
```

**하고나서, 위와 같이 값을 가져와서 쓰면 된다.**
