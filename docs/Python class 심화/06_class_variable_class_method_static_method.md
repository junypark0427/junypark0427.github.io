# 06. 클래스 변수, 클래스 메서드, 정적 메서드

인스턴스 변수와 클래스 변수를 구분하고 `@classmethod`, `@staticmethod`의 용도를 익힌다.

> **원본 자료:** `06_class_variable_class_method_static_method.py`  
> **편집 원칙:** 원본 코드와 주석을 그대로 수록하고, 수업에서 적은 핵심 설명을 본문에도 배치했다. 보충 설명은 최소한으로 덧붙였다.

## 📖 원본 코드와 수업 메모

### 클래스 변수와 인스턴스 변수

`self.name` 등은 인스턴스별 값이고, `Character.total_count`와 `Character.max_level`은 클래스 차원에서 관리하는 값이다.

**원본 수업 메모**

> - 핵심 질문 : 모든 Character가 공유하는 data는 어디에 저장하는가?
> - class variable : 모든 instance가 공유한다.
> - class 자체도 object이기 때문에 class의 __dict__에 저장된다.
> - instance variable (self.name) : 각 object가 개별적으로 소유
> - class variable (Character.total_count) : 모든 object가 소유

```python
# 핵심 질문 : 모든 Character가 공유하는 data는 어디에 저장하는가?

class Character:
    # class variable : 모든 instance가 공유한다.
    # class 자체도 object이기 때문에 class의 __dict__에 저장된다.
    total_count = 0
    max_level = 99

    def __init__(self, name, hp, level):
        # instance variable : 각 instance가 개별적으로 가지는 변수
        self.name = name
        self.hp = hp
        self.level = level
        Character.total_count += 1

    def __str__(self):
        return f"[Lv.{self.level} {self.name}, (HP : {self.hp})"
    
hero = Character("Arthur", 100, 5)
mage = Character("Merlin", 60, 7)
healer = Character("Aria", 50, 3)

# class variable은 class 이름으로 접근한다.
print(f"총 캐릭터 수 : {Character.total_count}") # 3
print(f"최대 레벨 : {Character.max_level}") # 99

# instance variable (self.name) : 각 object가 개별적으로 소유
# class variable (Character.total_count) : 모든 object가 소유
```

### @classmethod와 팩토리 메서드

첫 번째 인자로 클래스 객체 `cls`를 받아 공통 설정과 객체 생성(`create_character()`)을 처리한다.

**원본 수업 메모**

> - class method : 주로 class variable을 다루는 method
> - get_total_count = classmethod(get_total_count)의 간결한 문법
> - create_character = classmethod(create_character)
> - class method로 object를 생성하는 패턴 (Factory Method)

```python
class Character:
    total_count = 0
    max_level = 99

    def __init__(self, name, hp, level):
        self.name = name
        self.hp = hp
        self.level = level
        Character.total_count += 1

    def __str__(self):
        return f"[Lv.{self.level} {self.name}, (HP : {self.hp})"
    
    # class method : 주로 class variable을 다루는 method
    # get_total_count = classmethod(get_total_count)의 간결한 문법
    @classmethod
    def get_total_count(cls):
        return cls.total_count
    
    # set_max_level = classmethod(set_max_level)
    @classmethod
    def set_max_level(cls, value):
        if value < 1:
            print("최대 레벨은 1 이상 이어야 한다.")
            return
        cls.max_level = value
        print(f"최대 레벨이 {value}로 변경되었습니다.")

    # create_character = classmethod(create_character)
    # class method로 object를 생성하는 패턴 (Factory Method)
    @classmethod
    def create_character(cls, name, hp, level):
        return cls(name, hp, level)
    
# class method는 class 이름으로 호출한다.
print(f"총 캐릭터 수 : {Character.get_total_count()}") # 0

hero = Character("Arthur", 100, 5)
mage = Character("Merlin", 80, 7)
print(f"총 캐릭터 수 : {Character.get_total_count()}") # 2

# 최대 레벨 변경
Character.set_max_level(60)

# factory method를 이용하여 Character 생성
warrior = Character.create_character("전사", 200, 40)
wizard = Character.create_character("마법사", 80, 50)
print(warrior)
print(wizard)
print(f"총 캐릭터 수 : {Character.get_total_count()}") # 4
```

### @staticmethod와 메서드 비교

인스턴스나 클래스 데이터를 직접 받지 않는 독립적인 검증·계산 함수를 클래스 이름 아래에 묶는다.

**원본 수업 메모**

> - static method @staticmethod - decorator 사용
> - class나 instance의 내용과 별개인 독립적인 기능을 class 내부에 두고 관리하기 위한 문법이다.
> - self, cls와 같은 parameter를 갖지 않는다.
> - 주로 관련은 있지만 class/instance data가 필요하지 않는 유틸리티 함수에 주로 사용한다.
> - class method나 instance method, 혹은 외부의 함수로 대체가 가능하지만 몇 가지 이유가 있다.
> - 설계의 의도를 명확하게 한다. (그냥 유틸리티 함수)
> - instance method : 각 object의 data를 다룰 때
> - class method : class 전체에 적용되는 data를 다루거나 factory mehotd를 만들 때
> - static method : class와 관련된 유틸리티 함수를 묶어서 사용할 때

`@staticmethod`는 `self`나 `cls`를 자동으로 받지 않는다는 점이 핵심이다. 원본에 있는 메모리 관련 설명은 보조적인 이유로 이해하면 된다.

```python
# static method @staticmethod - decorator 사용
    # class나 instance의 내용과 별개인 독립적인 기능을 class 내부에 두고 관리하기 위한 문법이다. 
    # self, cls와 같은 parameter를 갖지 않는다.
    # 주로 관련은 있지만 class/instance data가 필요하지 않는 유틸리티 함수에 주로 사용한다.

# class method나 instance method, 혹은 외부의 함수로 대체가 가능하지만 몇 가지 이유가 있다. 
    # 설계의 의도를 명확하게 한다. (그냥 유틸리티 함수)
    # 수천 개의 instance를 생성하더라도 staticmethod는 instance마다 별도의 바인딩 object를 생성하지 않는다. (memory 절약)
    # 상속에서의 불필요한 간섭 방지

class Character:
    total_count = 0
    max_level = 99

    def __init__(self, name, hp, level):
        self.name = name
        self.hp = hp
        self.level = level
        Character.total_count += 1

    def __str__(self):
        return f"[Lv.{self.level} {self.name}, (HP : {self.hp})"
    
    # is_vaild_name = staticmethod(is_vaild_name)
    @staticmethod
    def is_vaild_name(name):
        if len(name) < 2 or len(name) > 20:
            return False
        return True
    
    # calculate_damage = staticmethod(calculate_damage)
    @staticmethod
    def calculate_damage(attack, defense):
        damage = attack - defense
        if damage < 0:
            damage = 0
        return damage
    
# static method는 class 이름으로 호출한다.
print(Character.is_vaild_name("A")) # False
print(Character.is_vaild_name("Arthur")) # True
print(Character.is_vaild_name("A" * 21)) # False

print(Character.calculate_damage(50, 30)) # 20
print(Character.calculate_damage(10, 30)) # 0

# 구분             / decorator      / 첫 번째 parameter  / 접근 가능 대상
# instance method / 없음            / self              / instance variable, class variable
# class method    / @classmethod   / cls               / class variable만
# static method   / @staticmethod  / 없음               / 독립 함수

# instance method : 각 object의 data를 다룰 때
# class method : class 전체에 적용되는 data를 다루거나 factory mehotd를 만들 때
# static method : class와 관련된 유틸리티 함수를 묶어서 사용할 때
```

## ✅ 핵심 정리 및 확인 사항

원본 코드는 수정하지 않았다. 아래 항목은 읽거나 실행할 때 확인하면 좋은 부분이다.

- 원본의 `is_vaild_name()`은 철자 그대로 보존했다. 일반적으로 `is_valid_name()`이라고 적는다.
- 클래스 메서드는 “클래스 변수만 접근 가능”으로 엄격히 제한되는 것은 아니다. `cls`로 클래스 속성을 다루거나 인스턴스를 생성할 수 있지만, 호출 객체의 `self`가 자동으로 전달되지는 않는다.
- 정적 메서드의 주된 장점은 관련 유틸리티를 논리적으로 묶는 것이다. 인스턴스 수에 따라 별도 메서드 코드가 무조건 복제되는 구조는 아니다.
