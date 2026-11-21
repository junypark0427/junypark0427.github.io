# 00. 객체지향 프로그래밍 복습

객체의 속성·메서드·생성자·참조 관계를 다시 확인하고, 클래스 상속의 기초를 복습한다.

> **원본 자료:** `00_review.py`  
> **편집 원칙:** 원본 코드와 주석을 그대로 수록하고, 수업에서 적은 핵심 설명을 본문에도 배치했다. 보충 설명은 최소한으로 덧붙였다.

## 📖 원본 코드와 수업 메모

### 객체 생성과 참조(reference)

`p3 = p1`은 객체를 복사하지 않고 같은 객체를 참조한다. 따라서 `p3.name`을 변경하면 `p1.name`으로 확인한 값도 달라진다.

```python


# 속성(Attribute)
# method
# constructor
# inheritance
# method override
# operator overloading
# object(instance) variable 
# object(instance) method


# game 
class Character:
    def __init__(self, name, hp, speed):
        self.name = name
        self.hp = hp
        self.speed = speed

    

# 등장인물 생성
p1 = Character("Peter", 100, 80)
p2 = Character("Paul", 150, 30)

p3 = p1 # 복사가 아니고 참조(reference)
p3.name = "coconut"

# 문제 : p1의 이름과 speed를 출력하고 이유를 설명
# 문제 : p2의 hp를 200 감소시키고 출력하시오


print(p1.name, p1.speed)

# if p2.hp - 200 < 0:
#     p2.hp = 0
#     print(f"{p2.name}이 dead")
# else:
#     p2.hp -= 200
# print(p2.hp)
```

### 인스턴스 메서드와 체력 감소

`reduce_hp()`는 `self.hp`를 갱신한다. 체력이 0 이하가 되면 0으로 맞추고 사망 메시지를 출력한다.

```python

class Character:
    def __init__(self, name, hp, speed):
        self.name = name
        self.hp = hp
        self.speed = speed


    def reduce_hp(self, damage):
        """입력받은 damage만큼 HP를 감소시키는 메서드"""
        self.hp -= damage  # 현재 체력에서 데미지를 뺌

        if self.hp <= 0:
            self.hp = 0
            print(f"💀 {self.name}이(가) 죽었습니다.")
            return self.hp
        else:
            print(f"📉 {self.name}의 hp가 감소했습니다. (현재 HP: {self.hp})")
            # 필요에 따라 현재 HP를 리턴할 수 있습니다.
            return self.hp
        

my_hero = Character("전사", 50, 10)

my_hero.reduce_hp(20)  # hp가 감소했습니다 출력
my_hero.reduce_hp(40)  # 죽었습니다 출력 및 HP 0 리턴
```

### 생성자와 클래스 상속

`Warrior`, `Wizard`는 `Character`를 상속하며, 별도 구현 없이도 부모의 생성자와 `attack()` 메서드를 사용한다. `isinstance()`로 클래스 관계를 확인한다.

**원본 수업 메모**

> - Constructor : 생성자
> - Warrior, Wizard 를 만들어 보자.
> - 기본 성격외에 별도의 능력을 가지고 있다.

```python
# Constructor : 생성자
# Warrior, Wizard 를 만들어 보자.
# 기본 성격외에 별도의 능력을 가지고 있다.
    # attack : 

class Character:
    def __init__(self, name, hp, speed):
        self.name = name
        self.hp = hp
        self.speed = speed
        print(f"새로운 캐릭터가 생성되었습니다.")
    
    def attack(self):
        print(f"{self.name}이 공격을 한다.")

    def get_status(self):
        print(f"{self.name}의 hp : {self.hp}")

class Warrior(Character): # Class inheritence
    pass

class Wizard(Character):
    pass

Arthur = Warrior("Arthur", 200, 100) # 생성자
Merlin = Wizard("Merlin", 50, 30)
Merlin.attack()
Arthur.attack()

print(isinstance(Arthur, Character))
print(isinstance(Arthur, Wizard))

# class inheritance method override
```

## ✅ 핵심 정리 및 확인 사항

원본 코드는 수정하지 않았다. 아래 항목은 읽거나 실행할 때 확인하면 좋은 부분이다.

- `p3 = p1`과 객체의 복사(`copy.copy()` 등)는 다르다.
- 같은 파일에서 `Character`를 여러 번 정의하는 것은 단계별 실습을 위한 것이다. 블로그에서도 예제를 순서대로 읽는 편이 좋다.
