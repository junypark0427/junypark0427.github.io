# 03. 특수 메서드(Dunder Method)

출력·비교·산술 연산·컨테이너 동작에 대응하는 `__메서드__`의 역할을 학습한다.

> **원본 자료:** `03_Special_method.py`  
> **편집 원칙:** 원본 코드와 주석을 그대로 수록하고, 수업에서 적은 핵심 설명을 본문에도 배치했다. 보충 설명은 최소한으로 덧붙였다.

## 📖 원본 코드와 수업 메모

### 특수 메서드의 개념

특수 메서드는 `__str__`, `__eq__`처럼 양쪽에 밑줄 두 개가 있는 메서드이다. `print(obj)`, `a == b`와 같은 문법에 연결된다.

**원본 수업 메모**

> - __method__ 형태의 special method를 말한다.
> - Python이 특정 상황에서 자동으로 호출되는 method이다.
> - ex) print(obj) -> __str__()을 호출하고, == -> __eq__()이 호출한다.
> - python에서 모든 class는 기본적으로 object class를 상속한다.
> - 따라서 이러한 dunder method는 object class에 정의된 method들이다.

```python
# Chapter 3 Special Method(Dunder Method)

    # __str__, __repr__ - 출력 관련
    # __eq__, __lt__ - 비교 연산
    # __add__, __mul__ - 산술 연산
    # __len__, __getitem__ - 컨테이너 동작
    # 도전과제 - 다른 소재 예제 (문제 형식)

# Dunder Method(double underscore method)

    # __method__ 형태의 special method를 말한다.
    # Python이 특정 상황에서 자동으로 호출되는 method이다.
    # ex) print(obj) -> __str__()을 호출하고, == -> __eq__()이 호출한다.


class Character:
    def __init__(self, name, hp, level):
        self.name = name
        self.hp = hp
        self.level = level

hero = Character("Arthur", 100, 5)
print(hero) # -> <__main__.Character object at
# 설명 :

    # python에서 모든 class는 기본적으로 object class를 상속한다.
    # 따라서 이러한 dunder method는 object class에 정의된 method들이다.

    # print(dir(object))를 통해서 확인할 수 있다.
```

### __str__와 __repr__

`__str__()`은 읽기 쉬운 표현, `__repr__()`은 개발자가 객체 상태를 확인하기 위한 표현을 제공한다. 리스트를 출력할 때는 원소의 `repr()` 표현이 사용된다.

**원본 수업 메모**

> - 설명 :
> - __str__ : 사용자에게 보여주는 읽기 쉬운 문자열로 사용(print(), str()에서 호출)
> - __repr__ : 개발자를 위한 정확한 문자열로 사용(print(), repr()에서 호출)
> - 설명 : -
> - print(hero) -> hero의 __str__ 호출
> - print(party) -> 리스트의 __str__ 호출 -> 각 요소는 __repr__ 호출

`print(객체)`는 주로 `__str__()`을 사용하고, 리스트 안에 들어 있는 객체를 표시할 때는 각 원소의 `__repr__()`이 사용된다.

```python
# __str__()과 __repr__()
class Character:
    def __init__(self, name, hp, level):
        self.name = name
        self.hp = hp
        self.level = level

    def __str__(self): # object 자체를 출력하면
        return f"Name : {self.name}, Level : {self.level}, hp : {self.hp}"
    
    def __repr__(self): # 출력 : 개발자가 debugging을 위해서 사용
        return f"Character('{self.name}', {self.level}, {self.hp})"
    
h = Character("Arthur", 100, 5)
print(f"{h}") 

print(repr(h))

# 설명 :
    # __str__ : 사용자에게 보여주는 읽기 쉬운 문자열로 사용(print(), str()에서 호출)
    # __repr__ : 개발자를 위한 정확한 문자열로 사용(print(), repr()에서 호출)

# 사용 예제 :

party = [
    Character("A", 100, 7),
    Character("B", 80, 7),
    Character("C", 60, 3)
]

for mem in party:
    print(mem)  # __str__() call 

# Name : A, Level : 7, hp : 100
# Name : B, Level : 7, hp : 80
# Name : C, Level : 3, hp : 60

print(party) # __repr__() call
# -> [Character('A', 7, 100), Character('B', 7, 80), Character('C', 3, 60)]

# 설명 : -
    # print(hero) -> hero의 __str__ 호출
    # print(party) -> 리스트의 __str__ 호출 -> 각 요소는 __repr__ 호출
```

### 비교 연산: __eq__, __lt__

같은 이름·레벨을 같다고 판정하고, 레벨 기준으로 크기를 비교하도록 연산 동작을 직접 정의한다. `sorted()`는 이러한 비교 메서드를 활용한다.

**원본 수업 메모**

> - 기본 == 비교는 메모리 주소를 비교
> - 만일 같은 속성의 object를 true로 평가하는 == 연산자를 만들려면 __eq__ method override 하면 된다
> - name과 level이 같으면 같은 Character로 판단한다.
> - Character 사이는 < 연산은 각 Character의 level로 판단한다.
> - 그런데 지금까지 사용했던 == 연산자는 어떻게 값을 비교하고 있었을까
> - 이것은 python 내부의 int, float, str등의 class가 __eq__ method를 override하고 있기 때문이다.

```python
# __eq__, __lt__ 등 연산

hero1 = Character("a", 10, 5)
hero2 = Character("a", 10, 5)
print(hero1 == hero2) # False -> 왜?

# 기본 == 비교는 메모리 주소를 비교
# 만일 같은 속성의 object를 true로 평가하는 == 연산자를 만들려면 __eq__ method override 하면 된다

# 사용 예제 :

    # name과 level이 같으면 같은 Character로 판단한다.
    # Character 사이는 < 연산은 각 Character의 level로 판단한다.


class Character:
    def __init__(self, name, hp, level):
        self.name = name
        self.hp = hp
        self.level = level

    def __str__(self): # object 자체를 출력하면
        return f"Name : {self.name}, Level : {self.level}, hp : {self.hp}"
    
    def __repr__(self): # 출력 : 개발자가 debugging을 위해서 사용
        return f"Character('{self.name}', {self.level}, {self.hp})"
    
    def __eq__(self, other):
        return self.name == other.name and self.level == other.level
    
    def __lt__(self, other):
        return self.level < other.level
    
hero1 = Character("A", 100, 3)
hero2 = Character("A", 30, 3)
hero3 = Character("B", 30, 4)

print(hero1 == hero2) # True
print(hero1 == hero3) # False
print(hero2 < hero3) # True 
print(hero3 < hero2) # False

party = [hero1, hero2, hero3]
sorted_party = sorted(party)

for mem in sorted_party:
    print(mem)

# 비교 연산자 method

    # __eq__ / ==
    # __lt__ / < (sorted()에서도 사용)
    # __le__ / <=
    # __gt__ / >
    # __ge__ / >=
    # __ne__ / !=


# 그런데 지금까지 사용했던 == 연산자는 어떻게 값을 비교하고 있었을까
# 이것은 python 내부의 int, float, str등의 class가 __eq__ method를 override하고 있기 때문이다.
```

### 산술 연산: __add__, __mul__

두 캐릭터를 `+`로 합성하고 `*`로 체력을 배수로 증가시키는 새 객체를 반환한다.

**원본 수업 메모**

> - __add__ / +
> - __sub__ / -
> - __mul__ / *
> - __truediv__ / /

```python
# __add__, __mul__ 등의 산술 연산 사용하기

class Character:
    def __init__(self, name, hp, level):
        self.name = name
        self.hp = hp
        self.level = level

    def __str__(self): # object 자체를 출력하면
        return f"Name : {self.name}, Level : {self.level}, hp : {self.hp}"
    
    def __repr__(self): # 출력 : 개발자가 debugging을 위해서 사용
        return f"Character('{self.name}', {self.level}, {self.hp})"
    
    def __eq__(self, other):
        return self.name == other.name and self.level == other.level
    
    def __lt__(self, other):
        return self.level < other.level
    
    def __add__(self, other):
        new_name = f"{self.name}_{other.name}"
        new_hp = self.hp + other.hp
        new_level = self.level + other.level
        return Character(new_name, new_hp, new_level)
    
    # * 연산 override : level n배 강화
    def __mul__(self, n):
        new_hp = self.hp * n
        return Character(self.name, new_hp, self.level)
    
arthur = Character("Arthur", 100, 5)
mage = Character("Mage", 30, 2)

aurage = arthur + mage
print(aurage)

arthurrrrrr = arthur * 100
print(arthurrrrrr)

# __add__ / +
# __sub__ / -
# __mul__ / *
# __truediv__ / /
```

### 컨테이너처럼 동작하는 Inventory

`__len__()`과 `__getitem__()`을 구현하면 사용자 정의 인벤토리에 `len(bag)` 및 `bag[index]`를 사용할 수 있다.

**원본 수업 메모**

> - __len__, __getitem__ object를 list처럼(흉내) len()과 []연산자 사용하기 (Emulating conatainer types)
> - Emulating container types : 사용자가 정의한 class가 list, tuple, dict과 같은 container 같은 역할을 할 수 있도록 만드는 것
> - 그 밖에 Emulating Container Type Method
> - __len__ / len()
> - __getitem__ / [], for문 순회
> - __setitem__ / [], write
> - __contains__ / in 연산자
> - __iter__ / iter() 함수, for문 순회

```python
# __len__, __getitem__ object를 list처럼(흉내) len()과 []연산자 사용하기 (Emulating conatainer types)
# Emulating container types : 사용자가 정의한 class가 list, tuple, dict과 같은 container 같은 역할을 할 수 있도록 만드는 것

# 아래 예제를 살펴보자

class Inventory:

    def __init__(self, owner):
        self.owner = owner
        self.items = []

    def add(self, item):
        self.items.append(item)
        print(f"{self.owner}가 '{item}'을 획득하였습니다. ")
    
    def __len__(self):
        return len(self.items)
    
    def __getitem__(self, index):
        if index < 0 or index >= len(self.items):
            print(f"{self.owner}의 인벤토리에 {index}칸은 없습니다.")
            return None
        
        return self.items[index]
    
    def __str__(self):
        return f"{self.owner}의 인벤토리 [{len(self)}칸]: {', '.join((self.items))}"
    
# 인벤토리 사용
bag = Inventory("Arthur")
bag.add("롱소드")
bag.add("체력 포션")
bag.add("마법 방패")

print(bag)

# 소유 아이템 개수
print(f"보유 아이템 수 : {len(bag)}")
print(f"첫 번째 아이템 : {bag[0]}")
print(f"10번째 아이템 : {bag[9]}")

# 그 밖에 Emulating Container Type Method
    # __len__ / len()
    # __getitem__ / [], for문 순회
    # __setitem__ / [], write
    # __contains__ / in 연산자
    # __iter__ / iter() 함수, for문 순회
```

## ✅ 핵심 정리 및 확인 사항

원본 코드는 수정하지 않았다. 아래 항목은 읽거나 실행할 때 확인하면 좋은 부분이다.

- 원본 `__repr__`의 인자 순서는 생성자의 `(name, hp, level)`과 다르게 `(name, level, hp)` 형식으로 작성되어 있다. 재생성 가능한 `repr` 형식을 의도한다면 확인이 필요하다.
- `Inventory.__getitem__()`은 음수 인덱스를 유효하지 않다고 판정한다. 기본 `list`와 달리 `bag[-1]`로 마지막 요소를 읽을 수 없다.
