# 01. 상속 심화: super(), MRO, Mixin

부모 클래스 초기화, 메서드 탐색 순서, 다중 상속 시 충돌 가능성과 Mixin 패턴을 이해한다.

> **원본 자료:** `01_inheritance_advanced.py`  
> **편집 원칙:** 원본 코드와 주석을 그대로 수록하고, 수업에서 적은 핵심 설명을 본문에도 배치했다. 보충 설명은 최소한으로 덧붙였다.

## 📖 원본 코드와 수업 메모

### 부모 생성자 직접 호출

`Character.__init__(self, ...)`처럼 부모의 생성자를 직접 호출할 수 있지만, 클래스 이름에 결합된 방식이다.

**원본 수업 메모**

> - 문제점 : Character class의 이름이 변경된다면....
> - super()를 사용하는 상속

```python


# 게임 캐릭터 프로그램을 소재로 상속에 관련된 내용을 알아보자.

# super(): parents를 가리킨다.

class Character:
    # 이름, hp
    def __init__(self, name, hp):
        self.name = name
        self.hp = hp

class Warrior(Character):
    def __init__(self, name, hp, armor):
        Character.__init__(self, name, hp)
        self.armor = armor

class MagicWarrior(Warrior):

    def __init__(self, name, hp, armor, mana):
        Warrior.__init__(self, name, hp, armor)
        self.mana = mana

# 문제점 : Character class의 이름이 변경된다면....
# super()를 사용하는 상속
```

### super()와 MRO

`super()`는 단순한 “부모 클래스의 이름”이 아니라 현재 클래스 다음의 **메서드 탐색 순서(MRO)**를 따른다. 단일 상속에서는 부모 기능 호출처럼 보인다.

**원본 수업 메모**

> - 상속을 하는 구조가 발생한다.
> - class의 구조, 다중 상속시 먼저 어떤 클래스가 상속되었는지 알 수 있음
> - 모든 class는 object라는 class를 가짐

```python
class Character:
    # 이름, hp
    def __init__(self, name, hp):
        self.name = name
        self.hp = hp
    
    def attack(self):
        print(f"{self.name} 기본 공격 ! ")

class Warrior(Character):
    def __init__(self, name, hp, armor):
        super().__init__(name, hp)
        self.armor = armor

    def attack(self):     # method override
        super().attack()
        print(f"{self.name}가 칼을 휘둘렀다")

class MagicWarrior(Warrior):

    def __init__(self, name, hp, armor, mana):
        super().__init__(name, hp, armor)
        self.mana = mana

arthur = Warrior("Arthur", 200, 50)
arthur.attack()

# 상속을 하는 구조가 발생한다.
print(Warrior.__mro__) # Method Resolution Order
# (<class '__main__.Warrior'>, <class '__main__.Character'>, <class 'object'>)

# class의 구조, 다중 상속시 먼저 어떤 클래스가 상속되었는지 알 수 있음
# 모든 class는 object라는 class를 가짐
```

### 다중 상속과 충돌 사례

`MagicWarrior(Warrior, Wizard)`처럼 여러 클래스를 상속하면 `super()` 호출 경로 및 생성자 인자 처리가 복잡해질 수 있다. 원본에는 이를 설명하기 위한 주석 처리 예제가 있다.

**원본 수업 메모**

> - 왜 오류?
> - MRO 때문에 parents class 정의된 정의된 super()가 가리키는 곳이 설계의 의도와 다른 곳이 될 수 있음.
> - 문제점 : 위 상속은 상속이 정상적으로 일어난다면 Character의 생성자가 중복 실행된다. (어떤 경우에는 이것이 문제일 수 있다.)

여기서 `Warrior.__init__()` 내부의 `super()`가 MRO를 따라 `Wizard.__init__()`으로 이어질 수 있다. `Wizard`가 요구하는 `mana` 인자가 전달되지 않아 생성자 인자 오류가 발생한다. 부모 생성자를 각각 직접 호출하는 방식으로 바꾸면 공통 조상의 초기화가 중복될 수도 있다.

```python
# 다중 상속(multiple inheritance)

    # 다중 상속에서 mro가 어떻게 형성되는지 알아보자

    # 전사이면서 마법도 사용하는 캐릭터

# class Character:
#     # 이름, hp
#     def __init__(self, name, hp):
#         self.name = name
#         self.hp = hp
    
#     def attack(self):
#         print(f"{self.name} 기본 공격 ! ")

# class Warrior(Character):
#     def __init__(self, name, hp, armor):
#         super().__init__(name, hp)
#         self.armor = armor

#     def attack(self):     # method override
#         super().attack()
#         print(f"{self.name}가 칼을 휘둘렀다")

# class Wizard(Character):
#     def __init__(self, name, hp, mana):
#         super().__init__(name, hp)
#         self.mana = mana

#     def attack(self):
#         print(f"{self.name}이 Fire ball!")

# class MagicWarrior(Warrior, Wizard):
#     def __init__(self, name, hp, armor, mana):
#         Warrior.__init__(self, name, hp, armor)
#         Wizard.__init__(self, name, hp, mana) # 직접 호출
#         print(f"{name}이 생성되었습니다")


# hero = MagicWarrior("D", 100, 50, 200)
# TypeError: Warrior.__init__() takes 4 positional arguments but 5 were given


# 왜 오류?
    # MRO 때문에 parents class 정의된 정의된 super()가 가리키는 곳이 설계의 의도와 다른 곳이 될 수 있음.

    # 문제점 : 위 상속은 상속이 정상적으로 일어난다면 Character의 생성자가 중복 실행된다. (어떤 경우에는 이것이 문제일 수 있다.)
```

### Mixin 패턴 활용

`Sword_mixin`, `Fire_mixin`을 별도 기능 클래스로 만들어 필요한 기술만 조합한다. Mixin은 생성자 상태를 최소화하면 조합하기 쉽다.

**원본 수업 메모**

> - 해결: Mixin Pattern
> - 충돌 위험이 있는 생성자나 method를 생략하는 방향으로 설계한다.
> - 기능 class 내부의 instance 변수 관리가 힘들어진다.
> - Mixin은 도구일 뿐, 적을수록 좋고 기능이 명확할 때만 사용

따라서 이 예제에서는 상태 초기화를 담당하는 `Character`와 공격 기능만 제공하는 Mixin을 분리한다. 다만 Mixin도 무분별하게 늘리면 구조가 복잡해질 수 있다.

```python
# 해결: Mixin Pattern

    # 충돌 위험이 있는 생성자나 method를 생략하는 방향으로 설계한다.


class Character:
    # 이름, hp
    def __init__(self, name, hp):
        self.name = name
        self.hp = hp

class Sword_mixin:
    def sword_attack(self):
        print(f"{self.name}이 칼을 휘두른다. {self.hp * 0.5}의 데미지")

class Fire_mixin:
    def fire_attack(self):
        print(f"{self.name}의 불맛보기 {self.hp * 0.8}의 데미지")

class Swimmixin:
    def swim(self):
        print(f"{self.name} is swimming")



# sub character
class Warrior(Sword_mixin, Character):
    def __init__(self, name, hp, armor):
        super().__init__(name, hp) # Character의 생성자만 호출
        self.armor = armor

class Wizard(Fire_mixin, Character):
    def __init__(self, name, hp, mana): 
        super().__init__(name, hp) # Character의 생성자만 호출
        self.mana = mana

class MagicWarrior(Sword_mixin, Fire_mixin, Character):
    def __init__(self, name, hp, armor, mana):
        super().__init__(name,  hp) # 우선순위 생성자가 없기 때문에 Character의 생성자를 가리키게 된다.
        self.armor = armor
        self.mana = mana

hero = MagicWarrior("Park", 100, 50 ,200)
print(MagicWarrior.mro())
# [<class '__main__.MagicWarrior'>, <class '__main__.Sword_mixin'>, <class '__main__.Fire_mixin'>, <class '__main__.Character'>, <class 'object'>]

hero.sword_attack()
hero.fire_attack()
print(f"{hero.name}의 armor : {hero.armor}, mana : {hero.mana}")

# 기능 class 내부의 instance 변수 관리가 힘들어진다.

# Mixin은 도구일 뿐, 적을수록 좋고 기능이 명확할 때만 사용

# ToDo list

    # Diamond Problem 정의
    # 우리 Code에서의 문제점을 이미지로 그려서 저장하기
    
```

## ✅ 핵심 정리 및 확인 사항

원본 코드는 수정하지 않았다. 아래 항목은 읽거나 실행할 때 확인하면 좋은 부분이다.

- Mixin 자체가 모든 다중 상속 충돌을 해결하는 것은 아니다. 실제 탐색 순서는 `Class.mro()`로 확인한다.
- 원본의 다중 상속 실패 예제는 주석 처리되어 있어 그대로 실행되지는 않는다.
