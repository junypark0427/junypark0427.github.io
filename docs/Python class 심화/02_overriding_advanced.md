# 02. 메서드 오버라이딩과 다형성

부모 메서드를 완전히 대체하는 방법과 부모 기능을 확장하는 방법을 구분하고, MRO와 다형성 예제를 이해한다.

> **원본 자료:** `02_overriding_advanced.py`  
> **편집 원칙:** 원본 코드와 주석을 그대로 수록하고, 수업에서 적은 핵심 설명을 본문에도 배치했다. 보충 설명은 최소한으로 덧붙였다.

## 📖 원본 코드와 수업 메모

### 완전한 오버라이딩

`Ninja.introduce()`와 `Bard.introduce()`는 부모의 `introduce()`를 각자의 방식으로 다시 정의한다.

**원본 수업 메모**

> - 소재 : 게임 캐릭터 성장 시스템
> - 핵심 질문 : 부모 클래스의 기능을 상속받았는데, 바꾸고 싶으면 어떻게 할까
> - 선택 사항
> - parents의 기능을 완전히 버리고 새로 작성
> - parents의 기능을 일부 사용하면서 새로 작성

```python
# 소재 : 게임 캐릭터 성장 시스템
# 핵심 질문 : 부모 클래스의 기능을 상속받았는데, 바꾸고 싶으면 어떻게 할까

# override : 

# 선택 사항
    # parents의 기능을 완전히 버리고 새로 작성
    # parents의 기능을 일부 사용하면서 새로 작성

class Character:
    # 속성 : name, hp
    def __init__(self, name, hp):

        self.name = name
        self.hp = hp

    def attack(self):
        print(f"{self.name} : basic attack") # 기본 공격
    
    def introduce(self):
        print(f"나는{self.name}이다")


class Ninja(Character):
    def __init__(self, name, hp):
        super().__init__(name, hp)

    def introduce(self): # 완전한 override
        print(f"...")

# 음유 시인
class Bard(Character):
    def __init__(self, name, hp):
        super().__init__(name, hp)

    def introduce(self):
        print(f"🎵나의 이름은 {self.name}~🎵")

# test : Ninja("병사"), Ninja("하야부시"), Bard("음유시인")
# 소개하는 code
party = [Ninja("병사", 100), Ninja("하야부시", 60), Bard("음유시인", 50)]

for member in party:
    member.introduce()

# ...
# ...
# 🎵나의 이름은 음유시인~🎵

#############################
```

### 부분적인 오버라이딩

`Warrior.level_up()`과 `Wizard.level_up()`은 `super().level_up()`으로 공통 레벨업을 수행한 뒤 각각 방어력·마나를 추가한다.

**원본 수업 메모**

> - 부분적 override

```python
# 부분적 override
class Character:
    # 속성 : name, hp
    def __init__(self, name, hp):

        self.name = name
        self.hp = hp
        self.level = 1
    
    def level_up(self):
        self.level += 1
        self.hp += 50
        print(f"{self.name} -> Lv.{self.level}! hp + 50")

class Warrior(Character):
    def __init__(self, name, hp, armor):
        super().__init__(name, hp)
        self.armor = armor

    def level_up(self):
        super().level_up()
        self.armor += 10
        print(f"    -> armor +10(현재 armor: {self.armor})")

# 마법사
class Wizard(Character):
    def __init__(self, name, hp, mana):
        super().__init__(name, hp)
        self.mana = mana

    def level_up(self):
        super().level_up()
        self.mana += 30
        print(f"    -> mana +30!(현재 mana: {self.mana})")

# test : Warrior("Arthur")
# test : Wizard("Merlin")

arthur = Warrior("Arthur", 100, 50)
merlin = Wizard("Merlin", 50, 100)

# level up
arthur.level_up()
merlin.level_up()


arthur.level_up() 
```

### 다이아몬드 상속과 MRO

`D(B, C)`가 `greet()`를 호출하면 MRO에 따라 `D → B → C → A` 순서로 메서드 호출이 이어진다.

**원본 수업 메모**

> - Diamond Problem에서 python의 MRO
> - D의 인사 B의 인사 A의 인사   B의 인사 끝

`D(B, C)`의 경우 `super()` 호출은 `D → B → C → A` 순서로 이어지며, 호출이 끝나면 역순으로 돌아오면서 나머지 출력이 실행된다.

```python
# Diamond Problem에서 python의 MRO

# D의 인사 B의 인사 A의 인사   B의 인사 끝


class A:
    def greet(self):
        print("A의 인사")

class B(A):
    def greet(self):
        print("B의 인사 시작")
        super().greet()
        print("B의 인사 끝")

class C(A):
    def greet(self):
        print("C의 인사 시작")
        super().greet()
        print("C의 인사 끝")

class D(B,C):
    def greet(self):
        print("D의 인사")
        super().greet()

d = D()
d.greet()
```

### 다형성(Polymorphism)

전사·마법사·힐러를 같은 리스트에 담고 각 객체의 `attack()`, `special()`을 호출해도 실제 실행되는 구현은 클래스마다 다르다.

**원본 수업 메모**

> - 다형성 (Polymerphism)
> - 다형성 이라고 한다.

```python
# 다형성 (Polymerphism)

class Character:
    # 속성 : name, hp
    def __init__(self, name, hp):

        self.name = name
        self.hp = hp
    
    def attack(self):
        print(f"{self.name} 공격")

    def special(self): # 특수 능력
        return f"{self.name} 특수 능력 없음"



class Warrior(Character):

    def attack(self):
        return (f"{self.name}이가 칼을 휘둘렀습니다")
    
    def special(self):
        return f"{self.name} : 검 배기 ! 25"

# 마법사
class Wizard(Character):
    def attack(self):
        return (f"{self.name}이 fire ball")

    def special(self):
        return f"{self.name}: 마나 회복"
    
class Healer(Character):
    def attack(self):
        return f"{self.name} : 지팡이 때리기"
    
    def special(self):
        return f"{self.name}: 전체 치유"
    
party = [Warrior("Arthur", 200), Wizard("Merlin", 80), Healer("Aria",100), Warrior("Gawain", 180)]

for mem in party:
    print(mem.attack())

for mem in party:
    print(mem.special())

# 다형성 이라고 한다.
```

## ✅ 핵심 정리 및 확인 사항

원본 코드는 수정하지 않았다. 아래 항목은 읽거나 실행할 때 확인하면 좋은 부분이다.

- 다이아몬드 예제의 실제 인사 출력에는 `C의 인사 시작`, `C의 인사 끝`도 포함된다. 원본 맨 위 출력 메모는 일부 과정을 생략했다.
- 마지막 다형성 예제에서 `Character.attack()`은 `print()`를 사용하지만 자식 클래스는 문자열을 `return`한다. 예제에서는 `print(mem.attack())`으로 문자열을 표시한다.
