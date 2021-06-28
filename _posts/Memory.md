# Memory Management

### Automatic Reference Counting(ARC)

Reference(참조 타입) 타입인 class는 힙(Heap) 메모리에 저장된다. 이 메모리는 자동으로 관리된다. ARC라고 불리는 이유는 참조 횟수를 세가면서 관리하는 기법이기 때문이다. 어떤 것을 가리키고 있는 포인터를 계속해서 탐지하고 있다가 참조 횟수가 0이 되면 즉시 그걸 놓아버리는 것이다. 이것은 가비지 컬렉션 (Garbage Collection)과는 다르다. 기본적으로 힙을 살펴보다가 표시하고 포인터로 가리키지 않는 것을 찾다가 휩쓸어 버리는 가비지 컬렉션 말이다. 이런 메모리 관리 방식은 간헐적으로 불규칙적일 뿐만 아니라 상당히 예측 불가능하다. 반면에 ARC는 완전히 예측하기 쉽다. 보통에 관해서는 ARC와 힙 메모리에 대해선 생각하지 않아도 된다. ARC에 영향을 줄 수 있는 몇가지 방법을 제외하고는 말이다. 아래 세 가지가 바로 그 몇가지 방법이다. 

#### Influencing ARC

- strong

  strong은 디폴트값이다. strong은 기본적인 Reference Counting(참조 집계) 방식이다. 기본적으로 strong 포인터는 힙 안에 있는 것들을 힙에 머물도록 강제하는 것이다. 그 포인터가 더이상 그것을 가르키지 않을 때까지 strong 포인터는 말그대로 힙 안에서 강하게(strong) 붙들고 있는다. 포인터를 만들었는데 모두 strong이라면 이를 깨끗이 청소하기 위해서 모든 포인터를 지워버려야한다. 

- weak

  weak은 그와 반대로 힙에 있는 것에 아무도 관심이 없다면 힙에서 제거할 수도 있다는 것이다. 그리고는 nil로 설정한다는 뜻이다. 이는 마치 힙에 있는 어떤 것을 가르키고는 있지만 그 안에 뭐가 있는지는 그만큼 관심이 없어서 사라져버리면 그냥 nil로 설정하겠다는 것이다. nil로 설정한다는 것은 옵셔널 포인터이어야 한다는 뜻이다. weak은 오직 옵셔널 타입의 참조 포인터에만 적용이 된다. 기본적으로 클래스에 대한 옵셔널 포인터이다. weak 포인터는 힙에 어떤 것도 저장해두지 않는다. 힙에 저장할 포인터는 strong 포인터에 달려있다. 좋은 weak 예시로는 Outlet이 있다. UILabel은 자동으로 weak이었다. 그 이유는 뷰 계층에서는 UILabel의 상위 뷰는 UILabel에 대해 strong 포인터를 갖고 있다. 그래서 outlet을 힙 안에 넣어두지 않아도 된다. 왜냐하면 뷰 계층에 따라 알아서 힙에 보관이 될 것으로 예상되기 때문이다. 그렇지만 뷰 계층이 그 UILabel을 제거해버린다면 outlet으로서 관심이 사라질 것이다. 그래서 그때는 outlet이 nil로 설정되게 두는 것이다. 이를 strong으로 바꾼다면 UILabel이 뷰 계층에서 벗어난다고 해도 힙에 계속 머물러 있을 것이다. 

- unowned

  이는 참조 횟수를 세지 말라는 것이다. 매우 위험하다. 거의 사용되지 않고 만약에 unowned 포인터가 있다면 참조 횟수를 추적하지 않겠다는 의미이다. 그래서 이 포인터는 항상 메모리에 있는 작은 공간을 가리킬 것이다. 포인터로 가리키는 것이 거기 있다는 것을 확인하는 것이 좋을 것이다. 더 이상 이 포인터를 사용하지 않을 때까지 지속해서 말이다. unowned 포인터가 있는데 참조를 나중에하고 참조한 것이 힙 밖으로 버려지게 되면 더 이상 strong포인터가 존재하지 않을테니 앱이 충돌하게 될 것이다. 메모리 참조 에러가 되는 것이다. 

위의 것들은 모두 변수를 선언할 때 필요한 것들이다. 
