### 📚 단순 연결 리스트
>💡**연결 리스트란?** 
>
원소 및 다음 노드로의 링크를 저장하고 있는 노드들로 이루어진 데이터 구조. 
>* 링크?
	노드가 컴퓨터 메모리상에서 어떤 주소로 저장되어있는가를 담고있는 정보.(포인터 = 메모리상에 주소)
>* 특징 
	1. get(i), set(i,x)를 상수 시간에 할 수 없음.
    2. 배열보다 동적인 구조를 가지고 있기 때문에 노드의 레퍼런스만 주어진다면, 노드의 추가와 삭제를 상수 시간에 할 수 있음.
    
>💡**단순 연결 리스트란?** 
>
노드들의 연속으로 이루어져 있고, 각 노드 u는 데이터 u.x와 다음 노드로의 링크인 u.next 값을 가짐. (마지막 노드 w의 w.next = nil)
>* queue연산 (add, remove) 과 stack연산 (push, pop) 을 단순 연결 리스트로 구현한 예.
![](https://velog.velcdn.com/images/codudals98/post/d4b5970d-7bd4-409b-8e8d-78ae4f60b7e1/image.png)
####
> * queue에서는 tail에서 add가 일어나고 head에서 remove가 일어나는 반면, stack은 pop, push모두 head 에서 일어남.
```py
initialize()
n <- 0 # node 수
head <- nil # head 없음
tail <- nil # tail 없음
```
```py
push(x)
# head쪽에 x추가
	u <- new_node(x) # x를 담는 새 node 생성
	u.next <- head # 새 node의 링크가 기존 head를 가리키도록 함
	head <- u # 새 node가 새로운 head가 됨
# 기존에 빈 리스트였다면 node가 하나밖에 없으므로 tail 역시 새로운 node로 지정
	if n = 0 then
    	tail <- u
    n <- n + 1 # node 수 증가
    return x
```
```py
pop()
# head를 리턴 후 제거
# 비어있는 리스트 이면 nil return
	if n = 0 then return nil
# 리스트가 비어있지 않다면 head의 원소 추출
	x <- head.x
# 기존 head의 다음 node를 head로 저장
	head <- head.next
    n <- n - 1
# node 수 감소 후 빈 리스트가 된 경우 tail에 Nil저장
	if n = 0 then
    	tail <- nil
	return x
```
```py
remove()
# stack의 pop과 똑같이 head에서 원소가 제거되므로
	return pop()
```
```py
add(x)
# tail쪽에 x추가
	u <- new_node(x) # x를 담는 새 node 생성
# 빈 리스트라면 새 node가 head됨
	if n = 0 then
    	head <- u
# 빈 리스트가 아니라면 tail뒤에 새 node를 추가
	else 
    	tail.next <- u
	tail <- u # 새 node가 tail이 됨
    n <- n + 1
return true
```
>🗣️ 요약 :
> *	**단순 연결 리스트**로 **stack**과 **(FIFO)queue**를 구현 할 수 있다. 
	* push, pop, add, remove 연산의 시간 복잡도는 여러 개의 원소 조각 없이 head나 tail 부분에서 하나의 원소만을 조작하기 때문에 O(1)
####
> *	단순 연결 리스트의 tail을 제거하는 연산은 시간 복잡도가 O(n). 
	* tail 바로 전 노드를 얻기 위해서는 head 부터 시작해서 링크를 바로 따라가는 수 밖에 없음.
####

### 📚 이중 연결 리스트
>💡**이중 연결 리스트란?** 
> * 노드가 다음 노드를 가리키는 링크인 u.next와 이전 노드를 가리키는 u.prev 값을 가짐. 
> * 관련 연산 구현 시 예외 처리의 어려움을 없애기 위해 dummy node가 존재.
>* queue연산 (add, remove) 과 stack연산 (push, pop) 을 단순 연결 리스트로 구현한 예.
![](https://velog.velcdn.com/images/codudals98/post/25670597-3dc7-451d-aefa-7d3b030d5533/image.png)
```py
initialize()
n <- 0 # 빈 DLList node를 만들어 dummy에 저장
dummy <- DLList.Node(nil)
dummy.prev <- dummy
dummy.next <- dummy
```
```py
get_node(i)
# i 번째 node를 리턴
# i, n - i 중 작은 값만큼 탐색이 일어나므로 O(min{i, n - i})
# i가 중앙보다 왼쪽 값이면 dummy 부터 순방향 탐색
	if i < n/2 then
    	p <- dummy.next
        repeat i times
        	p <- p.next
# i가 중앙보다 오른쪽 값이면 dummy부터 역방향 탐색
	else 
    	p <- dummy
        repeat n - i times
        	p <- p.prev
	return p
```
```py
get(i)
# i번째 node가 담고 있는 원소를 리턴
# get_node 호출이 일어나므로 O(min{i, n - i})
	return get_node(i).x
```
```py
set(i,x)
# i번째 node에 x 저장
# get_node 호출 외에는 상수 시간이므로 O(min{i, n - i})
	u <- get_node(i) # i번째 node를 얻은 뒤
	y <- u.x
    u.x <- x # node에 x wjwkd
    return y
```
```py
add_before(w,x)
# node w앞에 x추가
	u <- DLList.Node(x) # x를 담은 node u 생성
   	u.prev <- w.prev
    u.next <- w
    u.next.prev <- u
    u.prev.next <- u
    n <- n + 1
    return u
```
```py
add(i,x)
# i번째에 x가 오도록 추가
# get_node 호출 외에는 상수 시간이므로 O(min{i, n - i})
	add_before(get_node(i), x) # i 번째 node를 찾아서 그 앞에 x 추가
```
```py
remove_node(w)
# node w를 삭제
	w.prev.next <- w.next
    w.next.prev <- w.prev
    n <- n - 1
```
```py
remove(i)
# i번째 node 삭제
# get_node 호출 외에는 상수 시간이므로 O(min{i, n - i})
	remove_node(get_node(i))
```
>🗣️ 요약 :
> *	**이중 연결 리스트**는 **List인터페이스**(get, set, add, remove)를 구현 한다. 
	* _**get(i)**_, _**set(i,x)**_, _**add(i,x)**_, _**remove(i)**_는 _**O(min{i, n-i})**_의 시간복잡도를 가진다.
    * get_node(i) 부분응ㄹ 제외하면 이중 연결 리스트에 대한 다른 연산은 모두 상수 시간 복잡도를 가진다. 
	    * 이는 인덱스 기반 접근은 상수 시간이 들지만 추가나 삭제에 비용이 많이 드는 배열 기반 리스트와는 반대된다.
    * 이중 연결 리스트는 인덱스보다는 노드에 대한 레퍼런스가 주어지는 상황에서 더 유리하다.  
####