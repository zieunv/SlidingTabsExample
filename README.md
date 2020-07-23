# SlidingTabExample
UISlidingTabController.swift

세로만 되던 코드를 가로 화면까지 적용한 sliding tab example

### 코드 분석 과정 
``` swift
 func collectionView(_ collectionView: UICollectionView,
  layout collectionViewLayout: UICollectionViewLayout,
   sizeForItemAt indexPath: IndexPath) -> CGSize {
   
    if collectionView == collectionHeader {
      if tabStyle == .fixed {
        let spacer = CGFloat(titles.count)
        return CGSize(width: view.frame.width / spacer, height: CGFloat(heightHeader))
      } else {
        return CGSize(width: view.frame.width * 20 / 100, height: CGFloat(heightHeader))
      }
    }
		print("++ viewframe 값 \(view.frame)")

    return CGSize(width: view.frame.width, height: view.frame.height)
  }
```

해당 메서드에 print를 찍어보니 가로,세로일 때 frame size에 대한 값이 잘 나오고 있었지만 가로로 화면을 전환했을 때, 인덱스에 맞는 뷰의 영역이 제대로 잡히지 않았다. 







### 해결 코드

~~~
override func viewWillLayoutSubviews() {
    super.viewWillLayoutSubviews()
    collectionHeader.collectionViewLayout.invalidateLayout()
    collectionPage.collectionViewLayout.invalidateLayout()
    
    let pageOffset = CGPoint (
      x: Int((self.view.bounds.width)) * currentPosition ,
      y: 0
    )
    
    self.collectionPage.setContentOffset(pageOffset, animated: false)
    self.collectionPage.reloadData()
}
~~~
`current position` 변수는 각 탭의 인덱스를 가리키는 int형 변수이므로, (가로 -> 세로 또는 그 반대로) 화면이 전환된 이후 호출되는 `viewWillLayoutSubviews()`의 `pageOffset` 영역을 재설정해준 후

`self.collectionPage.setContentOffset(pageOffset, animated: false)`
를 이용하여 content view의 origin으로부터의 offset을 설정해준다. 



* setContentOffset 메서드에 대한 자료는 [ 요기 ](https://developer.apple.com/documentation/uikit/uiscrollview/1619400-setcontentoffset“appleDoc”) 


 
 
   
  