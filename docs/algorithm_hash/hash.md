---
layout: default
title: Algorithm 해시
nav_order: 11
has_children: true
permalink: /docs/algorithm_hash
---


# 해시

해시 알고리즘 문제 해결 전략  

해시는 Key-value쌍으로 데이터를 저장하는 자료구조 입니다.

---

### 코딩 인터뷰 완전분석

해시 테이블은(hash table)은 효울적인 탐색을 위한 자료구조로서 키(key)를 값(value)에 대응시킨다.  

```scss
 hash table = LinkedList + hashCode()
```

키(문자열 혹은 다른 어떤 자료형도 가능하다)와 값을 해시테이블에 넣을 때는 다음의 과정을 거친다.

1. 처음엔 키의 해시 코드를 계산한다.  
키의 자료형은 보통 int 혹은 long이 된다.  
키의 개수는 무한한데 반해 int의 개수는 유한하기 때문에 서로 다른 두 개의 키가 같은 해시 코드를 가리킬 수 있다는 사실을 명심해라.  

2. 그 다음엔 `hash(key) % array_length`와 같은 방식으로 해시 코드를 이용해 배열의 인덱스를 구한다.  
물론 서로 다른 두 개의 해시 코드가 같은 인덱스를 가리킬 수 도 있다.  

3. 배열의 각 인덱스에는 키와 값으로 이루어진 연결리스트가 존재한다.  
키와 값을 해당 인덱스에 저장한다.  
`충돌`에 대비해서 반드시 연결리스트를 이용해야한다.
여기서 `충돌`이란 서로 다른 두 개의 키가 같은 해시 코드를 가리키거나 서로 다른 두 개의 해시 코드가 같은 인덱스를 가리키는 경우를 말한다.  

```scss
충돌을 줄이기 위한 2가지 방법  

1. 체이닝 (연결리스트 사용)
2. 개방형 주소 방법
    1) 선형 조사
    2) 이차원 조사
    3) 이중 해시
```

키에 상응하는 값을 찾기 위해선 다음의 과정을 반복해야 한다.  
주어진 키로부터 해시 코드를 계산하고, 이 해시 코드를 이용해 인덱스를 계산한다.  
그 다음엔 항상 키에 상응하는 값을 연결리스트에서 탐색한다.  

충돌이 자주 발생한다면, 최악의 경우의 수행 시간(worst case runtime)은 `O(N)`이 된다(N은 키의 개수).  

하지만 일반적으로 해시에 대한 이야기할 때는 충돌을 최소화하도록 잘 구현된 경우를 가정하는데 이 경우에 탐색 시간은 `O(1)`이다.

![](/assets/images/algorithm/hash/hash.jpeg)  

---

### 학교수업


    * 해싱
        * 해싱 : 키를 사용하여 데이터가 저장된 테이블의 인덱스를 알아내는 기법
        * 키로 인덱스된 테이블, 즉 해시 테이블(hash table)에 아이템을 저장하는 기법
        * 검색트리는 일년의 비교연산을 통하여 검색을 하지만, 해시 테이블은 키에 따라 즉시 검색 위치가 결정된다. => 상수시간 검색
        * 삽입 / 삭제를 동반하는 동적검색에 적절  
        * 검색 / 삽입 / 삭제만 가능하며, 검색트리와 같은 추가적인 이점이 없다.  
    
    
    * 해시 함수
        * 해시 함수(hash function)
            * 키 값을 입력으로 받아 해시 테이블 상의 주소(index)를 리턴하는 함수 : h( )
            * ✨해시코드(hash code) + ✨압축함수(compression function)

        * 좋은 해시 함수의 조건
            * 계산이 빠르고 간편해야 한다.
            * 가능한 충돌 발생이 적어야 한다. 즉, 최대한 랜덤하게 키들을 테이블에 퍼지게 저장!
            * ex) 특정 교과목을 수강하는 학생들의 학번을 키로 하는 검색
			    - Bad : 앞 네 자리 혹은 중간 두 자리
			    - Better : 마지막 세 자리

        * ✨해시코드 (hash code)
            * 키 값에 정수(-2^-31 ~ 2^31 -1)를 매핑(대응)시키는 함수 : h1()
            * 타입 별로 서로 다른 접근방법이 필요
			    - Java : 모든 타입 클래스는 hashCode() 메소드를 상속받는다. (primitive = > wrapper)
            * 수치형 : 키의 비트 표현식을 정수로 변환(integer cast)
			    - 비트자리수가 큰 경우는 합 (addtion) 혹은 배타적 논리합 (exclusive-or) 사용
            * 문자열 : 가변길이이며, 문자의 순서가 중요

![](/assets/images/algorithm/hash/polynomialHashcode.png)  
			
![](/assets/images/algorithm/hash/hashCode.png)  
		    

    * 사용자 정의 타입(user-defined types)
		- 각 필드별로 해시코드를 생성한 후, polynomial hashcode 사용
		- 단, 초기 해시값은 0이 아닌 정수 상수로!

    * ✨압축함수(compression function) 혹은      모듈라 해싱(modular hashing)

        * 해시코드에 테이블의 인덱스를 매핑시키는 함수 : h2()
        * 해시 테이블의 크기가 m인 경우, h2 : hashcode -> {0, 1, ..., m-1 }
        * m은 보통 소수(prime number)를 사용하거나, 2의 거듭제곱을 사용
        * Division 방법 : 해시코드 x에 대하여
			- x가 양수이면 h2(x) = x%m, 음수이면 h2(x) = ㅣxㅣ%m
			- 더 정확한 방법 : h2(x) = (x & 0x7fffffff) % m, "bitwise-and"
        * MAD(Multiply, Add and Divide) 방법
			- h2(x) = (ax + b)%m, where a>0, b>0, a%m != 0

    * ✨해시값(hash value)
        * h(k) = h2(h1(k))을 키 값 k의 해시값이라고 한다.
        * 키 값 k를 갖는 데이터는 해시 테이블의 인덱스 h(k) 위치에 저장된다.
        * 불가피하게 충돌(collision)이 발생한다. h(k1) = h(k2), k1 != k2
        * 충돌 해결 방법 : 체이닝(chaining), 개방 주소 방법(open addressing)

    * 적재율(load factor)

        * 해시 테이블에 원소가 차 있는 비율
        * 테이블의 크기가 m, 저장된 데이터의 수가 n이면, a=n/m  
        
        
---


    * ㄱ. 체이닝(chaining)
        * 같은 주소로 들어온 원소들을 해당 헤더 아래 연결 리스트로 매달아 관리한다.
        * 해시 테이블의 각 주소가 연결 리스트의 헤더 역할
        * 적재율이 1 이상인 경우도 사용할 수 있지만, 추가공간이 필요한 단점이 있다.
        * 체이닝의 적재율은 0.9 이하가 바람직하다.
        * 예) m=13인 경우, 55, 13, 42, 70, 43, 44, 3, 94, 47, 74, 39, 86, 76, 40을 삽입  
    
![](/assets/images/algorithm/hash/chaining.png)  


    * ㄴ. 개방 주소 방법(open addressing)
        * 추가공간을 허용하지 않고, 주어진 테이블 공간에서 해결한다.
        * 새로운 키가 충돌하면, 다음 빈 슬롯을 찾아 저장한다.
        * 다음 주소를 결정하는 대표적인 방법들 (i는 충돌 횟수)
            * 선형조사(linear probing) : hi(x) = (h(x) + i)%m
            * 이차원 조사(quadratic probing) : hi(x) = (h(x)+i*d(x))%m
        * 예) m = 13 인 경우, 25,13,16,15,7,28,31,20,1,26을 삽입, 단 d(x)=7-(x%7)  
        

![](/assets/images/algorithm/hash/openAddressing.png)  
￼

            * 더블 해싱의 성능이 가장 뛰어나다.
            * 개방 주소 방식을 사용할 경우, 원소를 삭제할 때는 반드시 원래는 원소가 있던 자리였음 표시해 주어야 한다.
        * 해시 테이블 크기의 동적 재조정(dynamic resizing)
            * 개방주소방법은 적재율이 높아지면 효율이 급격히 떨어진다.
            * 적재율이 1을 넘으면, "Table Overflow"
            * 적재율에 대한 임계값을 설정한 후, 그 값을 넘으면 해시 테이블의 크기를 두 배로 키우고 모든 원소를 다시 해싱(rehashing)하여야 한다.
            * 적재율 0.5 이하인 경우 체이닝보다 개방주소 방법이 낫다.
        * 해시 테이블 역시 시스템 심볼 테이블에 광범위하게 사용
            * Java : java.util.HashMap, java.util.IdentityHashMap
            
[관련 링크1](https://github.com/jobhope/TechnicalNote/blob/master/data_structure/Hashing.md){: .btn .fs-5 .mb-4 .mb-md-0 }
[관련 링크2](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/DataStructure#hash-table){: .btn .fs-5 .mb-4 .mb-md-0 }
[관련 링크3](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Data%20Structure/Hash.md){: .btn .fs-5 .mb-4 .mb-md-0 }  

[관련 영상](https://www.youtube.com/watch?v=Vi0hauJemxA&t=155s){: .btn .fs-5 .mb-4 .mb-md-0 }
