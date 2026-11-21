# 04. 캡슐화와 Getter/Setter

`public`, 관례적 `protected`, 이름 맹글링이 적용된 속성을 구분하고 값 검증에 Getter/Setter를 활용한다.

> **원본 자료:** `04_encapsulation.py`  
> **편집 원칙:** 원본 코드와 주석을 그대로 수록하고, 수업에서 적은 핵심 설명을 본문에도 배치했다. 보충 설명은 최소한으로 덧붙였다.

## 📖 원본 코드와 수업 메모

### 속성 접근과 Name Mangling

`name`, `_hp`, `__level`의 차이를 확인한다. Python의 `__level`은 이름 맹글링이 적용되지만 완전한 접근 차단을 뜻하지는 않는다.

**원본 수업 메모**

> - public - 접근 가능
> - protected - 접근 가능, 관례상 외부에서 사용하지 않는다.
> - private - 접근 불가
> - Name Mangling이란?
> - __로 시작하는 속성은 Python이 내부적으로 이름을 변경한다.
> - self.__level -> self._Character__level로 변경한다.
> - Name Mangling은 완벽한 속성 보호는 아니다.
> - 실수로 접근하는 것을 방지하는 정도로 이해한다.

`_hp`는 관례상 비공개이고, `__level`은 이름이 바뀌는 방식이므로 완전한 접근 차단은 아니다.

```python


# Encapsulation : 캡슐화

class Character:
    def __init__(self, name, hp, level):
        self.name = name   # 자유
        self._hp = hp      # 하지 말기로 해요
        self.__level = level # 

    def __str__(self):
        return f"[Lv.{self.__level}] {self.name} (HP : {self._hp})"
    
hero = Character("Arthur", 100, 5)
print(hero) # [Lv.5] Arthur (HP : 100)
# 어떤 속성(attribute)을 가지고 있는가? name, hp, level
hero._hp = -2
# hero.__level = 30
print(hero)

# public - 접근 가능
print(hero.name) 

# protected - 접근 가능, 관례상 외부에서 사용하지 않는다.
print(hero._hp)

# private - 접근 불가
# print(hero.__level) # AttributeError

# Name Mangling이란?
    # __로 시작하는 속성은 Python이 내부적으로 이름을 변경한다.
    # self.__level -> self._Character__level로 변경한다.


hero._Character__level = 4
print(hero._Character__level)

# Name Mangling은 완벽한 속성 보호는 아니다.
# 실수로 접근하는 것을 방지하는 정도로 이해한다.
# Python의 철학













# 외부에서 속성 변경 가능


# 모든 속성이 public이라 외부에서 아무 제한 없이 변경이 가능
```

### Getter와 Setter

`get_hp()`, `set_hp()` 등을 두어 값을 읽고 변경한다. `set_hp()`는 HP가 0~999 범위 밖이면 값을 변경하지 않는다.

**원본 수업 메모**

> - 설명 :
> - getter : 숨겨진 속성을 읽을 수 있게 해준다.
> - setter : 값을 변경할 때 유효성 검사를 유도할 수 있다.
> - 외부에서 직접 속성에 접근하는 것을 막아 프로그램의 안정성을 높인다.

```python
class Character:
    def __init__(self, name, hp, level):
        self.name = name   # 자유
        self.__hp = hp      
        self.__level = level # 

    def __str__(self):
        return f"[Lv.{self.__level}] {self.name} (HP : {self.__hp})"
    
    # getter : 속성값을 읽는 method
    def get_hp(self):
        return self.__hp
    
    def get_level(self):
        return self.__level
    
    # setter : 속성값을 변경(write)하는 method(유효성 검사 포함)
    def set_hp(self, value):
        if value < 0:
            print("HP는 0보다 작을 수 없음")
        elif value > 999:
            print("HP는 999를 초과할 수 없음")
        else:
            self.__hp = value

    def set_level(self, value):
        if value < 1:
            print("레벨은 1보다 작을 수 없음")
            return
        self.__level = value


    
hero = Character("Arthur", 100, 5)
# getter로 읽어오기
print(f"HP : {hero.get_hp()}")
print(f"Level : {hero.get_level()}")

# setter로 값 변경 (유효성 검사가 동작)
hero.set_hp(150)
print(hero) # [Lv.5] Arthur (HP : 150)

hero.set_hp(-99) # HP는 0보다 작을 수 없음
print(hero) # [Lv.5] Arthur (HP : 150)

hero.set_level(0) # 레벨은 1보다 작을 수 없음
print(hero) # [Lv.5] Arthur (HP : 150)

# 설명 :
    # getter : 숨겨진 속성을 읽을 수 있게 해준다.
    # setter : 값을 변경할 때 유효성 검사를 유도할 수 있다.
    # 외부에서 직접 속성에 접근하는 것을 막아 프로그램의 안정성을 높인다.
```

### 캡슐화된 전투 시스템

피격과 공격을 객체 메서드로 다루며, HP 하한을 0으로 유지하고 `is_alive()`로 생존 여부를 확인한다.

```python
# Encapsulation이 적용된 전투 시스템

class Character:
    def __init__(self, name, hp, level, attack_power):
        self.name = name   # 자유
        self.__hp = hp      
        self.__level = level 
        self.__attack_power = attack_power

    def __str__(self):
        return f"[Lv.{self.__level}] {self.name} (HP : {self.__hp})"
    
    # getter : 속성값을 읽는 method
    def get_hp(self):
        return self.__hp
    
    def get_level(self):
        return self.__level
    
    # setter : 속성값을 변경(write)하는 method(유효성 검사 포함)
    def set_hp(self, value):
        if value < 0:
            print("HP는 0보다 작을 수 없음")
        elif value > 999:
            print("HP는 999를 초과할 수 없음")
        else:
            self.__hp = value

    def set_level(self, value):
        if value < 1:
            print("레벨은 1보다 작을 수 없음")
            return
        self.__level = value

    def is_alive(self):
        return self.__hp > 0
    
    def take_damage(self, damage):
        if damage < 0:
            print("damage는 음수일 수 없습니다.")
            return
        self.__hp -= damage
        if self.__hp < 0:
            self.__hp = 0
        print(f"{self.name}이 {damage}의 Damage를 받았습니다. (남은 HP : {self.__hp})")

    def attack(self, other):
        print(f"{self.name}이 {other.name}을 공격합니다.")
        other.take_damage(self.__attack_power)

# Character 생성
hero = Character("Arthur", 100, 25, 30)
monster = Character("Goblin", 50, 10, 50)

# 전투
hero.attack(monster) # Arthur이(가) Goblin을(를) 공격
monster.attack(hero) # Goblin이(가) Arthur을(를) 공격
hero.attack(monster) # Arthur이(가) Goblin을(를) 공격

print(hero)
print(monster)
print(f"Goblin 생존 여부 : {monster.is_alive()}") # Goblin 생존 여부 : False
```

## ✅ 핵심 정리 및 확인 사항

원본 코드는 수정하지 않았다. 아래 항목은 읽거나 실행할 때 확인하면 좋은 부분이다.

- `_hp`는 실제 접근 제한자가 아니라 “외부에서 직접 사용하지 말자”는 개발 관례이다.
- `__level`도 `_Character__level`처럼 이름 맹글링된 명칭으로 접근할 수 있으므로 보안 장치로 간주하면 안 된다.
