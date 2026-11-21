# 05. Property와 Decorator

기존 Getter/Setter에서 `property()`, `@property`, `@속성.setter`로 발전하는 흐름을 익힌다.

> **원본 자료:** `05_property와 Decorator.py`  
> **편집 원칙:** 원본 코드와 주석을 그대로 수록하고, 수업에서 적은 핵심 설명을 본문에도 배치했다. 보충 설명은 최소한으로 덧붙였다.

## 📖 원본 코드와 수업 메모

### 기존 Getter/Setter 방식

`get_hp()`, `set_hp()`처럼 메서드 호출로 속성 값을 읽고 바꾸는 기본 구현이다.

```python
# Chapter 4 
# 1.
class Character:
    def __init__(self, name, hp):
        self.name = name
        self.__hp = hp

    def get_hp(self):
        return self.__hp
    
    def set_hp(self, value):
        if value < 0:
            self.__hp = 0
        else:
            self.__hp = value
    
hero = Character("Arthur", 100)

print(hero.get_hp())
hero.set_hp(150)
print(hero.get_hp()) # 150 
```

### property()로 속성처럼 접근하기

`hp = property(get_hp, set_hp)`를 선언하면 외부에서는 `hero.hp`로 값을 읽고 대입할 수 있다.

**원본 수업 메모**

> - 2. property class - attribute처럼 쓰는 getter, setter
> - Python의 property는 object에서 일반 변수처럼 직관적으로 접근할 수 있게 해주는 내장 class이다.
> - getter, setter, deleter로 사용할 method를 등록하여 instance를 생성하면 object에서 변수처럼 사용할 수 있다.
> - get_hp(self)와 set_hp(self, hp)를 hp라는 이름의 property object로 생성하며 hero object에서 마치 변수처럼 사용한다.
> - Python에서는 이러한 문법을 좀 더 간결하게 사용할 수 있는 @property 문법을 제공한다.

```python
# 2. property class - attribute처럼 쓰는 getter, setter
# Python의 property는 object에서 일반 변수처럼 직관적으로 접근할 수 있게 해주는 내장 class이다. 
# getter, setter, deleter로 사용할 method를 등록하여 instance를 생성하면 object에서 변수처럼 사용할 수 있다. 
class Character:
    def __init__(self, name, hp):
        self.name = name
        self.__hp = hp

    def get_hp(self):
        return self.__hp
    
    def set_hp(self, value):
        if value < 0:
            self.__hp = 0
        else:
            self.__hp = value
    
    hp = property(get_hp, set_hp)

hero = Character("Arthur", 100)
hero.hp = 200
print(hero.hp) # 200

# get_hp(self)와 set_hp(self, hp)를 hp라는 이름의 property object로 생성하며 hero object에서 마치 변수처럼 사용한다.
# Python에서는 이러한 문법을 좀 더 간결하게 사용할 수 있는 @property 문법을 제공한다.
```

### @property 데코레이터

메서드 앞에 `@property`와 `@hp.setter`를 붙이는 더 간결한 문법을 사용한다.

**원본 수업 메모**

> - 3. decorator @property
> - decorator를 사용하면 위 code를 훨씬 간결하게 사용할 수 있다.
> - getter, setter, deleter로 사용할 method의 앞에 @~~를 붙이고 method 이름을 변수처럼 사용할 이름으로 통일하면 된다.
> - 이러한 문법을 decorator라고 한다.
> - 만일 setter를 정의하지 않으면 읽기 전용 attribute가 된다.

`@property`는 호출할 때 괄호를 쓰지 않고 `hero.hp`처럼 접근하게 해준다. 값을 대입할 때는 `@hp.setter`로 지정한 함수가 실행된다.

```python
# 3. decorator @property
# decorator를 사용하면 위 code를 훨씬 간결하게 사용할 수 있다. 

class Character:
    def __init__(self, name, hp):
        self.name = name
        self.__hp = hp

    @property
    def hp(self):
        return self.__hp
    
    @hp.setter
    def hp(self, hp):
        self.__hp = hp
    
    hero = Character("Arthur", 100)
    hero.hp = 200
    print(hero.hp)

# getter, setter, deleter로 사용할 method의 앞에 @~~를 붙이고 method 이름을 변수처럼 사용할 이름으로 통일하면 된다. 
# 이러한 문법을 decorator라고 한다.
# 만일 setter를 정의하지 않으면 읽기 전용 attribute가 된다. 
```

### 읽기 전용 속성과 전투 예제

`attack_power`, `is_alive`는 읽기 전용 프로퍼티이며 `hp`에 대해서는 0~999 범위 제한을 적용한다.

**원본 수업 메모**

> - setter를 통해서 자동 유효성 검사를 할 수 있다.

```python
class Character:
    def __init__(self, name, hp, attack_power):
        self.name = name
        self.__hp = hp
        self.__attack_power = attack_power

    def __str__(self):
        return f"{self.name} (HP : {self.hp})"
        
    @property
    def hp(self):
        return self.__hp
    
    @hp.setter
    def hp(self, hp):
        if hp < 0:
            self.__hp = 0
        elif hp > 999:
            self.__hp = 999
        else:
            self.__hp = hp
    
    @property                   # 읽기 전용
    def attack_power(self):
        return self.__attack_power
    
    @property                   # 읽기 전용
    def is_alive(self):
        return self.hp > 0
    
    def take_damage(self, damage):
        if damage < 0:
            print("Damage는 음수일 수 없습니다.")
            return
        
        # setter를 통해서 자동 유효성 검사를 할 수 있다.
        self.hp = self.hp - damage
        print(f"{self.name}이 {damage}의 Damage를 받았습니다! (남은 HP : {self.hp})")

    def attack(self, other):
        print(f"{self.name}이 {other.name}을 공격합니다.")
        other.take_damage(self.attack_power)

# Character 생성
hero = Character("Arthur", 100, 25)
monster = Character("Goblin", 50, 10)

# 전투
hero.attack(monster)
monster.attack(hero)
hero.attack(monster)

print(hero)
print(monster)
print(f"Goblin 생존 여부 : {monster.is_alive}") # 변수처럼 사용
```

### 중첩 함수(Nested Function)

`calculate_price()`는 `order_coffee()` 안에 정의된 함수로, 바깥쪽 함수 인자인 `size`를 참조한다.

```python
# nested function
def order_coffee(size):
    def calculate_price(base):
        if size == "tall":
            return base
        elif size == "grande":
            return base + 500
        elif size == "venti":
            return base + 1000
        return base
    
    price = calculate_price(4500)
    print(f"{size} 사이즈 : {price}원")

order_coffee("tall")
order_coffee("grande")
order_coffee("venti")
# calculate_price("tall")
```

## ✅ 핵심 정리 및 확인 사항

원본 코드는 수정하지 않았다. 아래 항목은 읽거나 실행할 때 확인하면 좋은 부분이다.

- 원본 세 번째 예제의 `hero = Character(...)` 테스트 코드는 `class Character:` 본문 안에 들여쓰기되어 있다. 따라서 새로운 클래스 정의가 끝난 뒤의 테스트가 아니라 **클래스 본문 실행 시점**에 평가된다. 예제를 별도로 실행하려면 테스트 코드를 클래스 밖으로 옮겨야 한다.
- 원본 세 번째 예제의 setter는 음수 HP를 검증하지 않지만, 뒤에 나오는 전투 예제에서는 0~999 제한을 추가한다.
