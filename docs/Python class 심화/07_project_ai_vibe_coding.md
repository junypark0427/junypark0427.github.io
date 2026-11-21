# 07. 실전 프로젝트: 텍스트 RPG와 AI 바이브코딩

상속·캡슐화·프로퍼티·다형성·클래스 메서드를 결합해 턴제 RPG를 구현하고 검증한다.

> **원본 자료:** `07_project_ai_vibe_coding.py`  
> **편집 원칙:** 원본 코드와 주석을 그대로 수록하고, 수업에서 적은 핵심 설명을 본문에도 배치했다. 보충 설명은 최소한으로 덧붙였다.

## 📘 원본 프로젝트 기획 및 검증 기준

### 목표

- 지금까지 배운 class 개념을 종합하여 하나의 프로젝트를 완성한다.  
- AI에게 정확한 요구사항을 전달하는 방법을 익힌다.  
- AI가 만든 코드를 분석하고 검증할 수 있다.  

### 프로젝트 1: 텍스트 RPG 전투 게임

- Game Flow
- 핵심 class 직접 구현
- 직접 구현한 class 구조와 게임 흐름 등을 이용하여 AI에게 전달할 요구사항 작성
- 직접 작성한 class 관련 점검표 받기(AI) -> Test Code
- game flow에 맞는 battle code 받기
- battle code 검증 (Verification Checklist)
- Test
- upgrade 요구: 과제

---

### Game Flow

1. Player 이름 입력
2. Character의 직업을 선택 (1. Warrior, 2. Wizard, 3. Healer)
3. Monster가 random으로 등장
4. Turn-Based Combat (턴제 전투)
    - Player Turn: 1. 공격, 2. 특수 공격, 3. 회복 아이템 사용
    - Monster Turn: random 공격
5. Monster를 처치하면 경험치를 획득, 다음 Monster가 등장
6. 3마리 Monster 처치 시 승리, Player 사망 시 Game Over
7. 전투 종료 후 결과 출력

#### 기타 주의 사항

- 'input()'으로 사용자 이름 입력받기
- 잘못된 입력에 대한 예외 처리 포함
- 전투 상황을 읽기 쉽게 출력

---

### 핵심 class 직접 구현

AI에게 요구사항을 전달하기 전에 핵심 class를 직접 구현한다. 

* **`Character` (최상위 부모 클래스)**
* **캡슐화 및 프로퍼티**: 체력, 공격력, 방어력, 레벨, 경험치 등의 속성을 Private(`__`) 변수로 은닉하고 `@property` 및 Setter를 통해 유효성 검사 적용
* **기본 메커니즘**: 피격(`take_damage`), 기본 공격(`attack`), 경험치 획득 및 레벨업(`gain_exp`) 구현
* **클래스 및 정적 메서드**: 생성된 전체 캐릭터 수를 추적하는 `total_count`(`get_total_count`)와 최소 데미지 보장 공식인 `calculate_damage` 구현


* **직업별 자식 클래스 (`Warrior`, `Wizard`, `Healer`)**
* **상속 및 다형성**: `Character`를 상속받아 직업별 전용 자원(전사의 분노, 마법사/힐러의 마나) 추가
* **메서드 오버라이딩**: 일반 공격(`attack`) 및 특수 능력(`special`)을 직업 컨셉에 맞춰 재정의 (전사: 2배 피해/분노 폭발, 마법사: 마법 화살/파이어볼, 힐러: 빛의 화살/자가 회복)


* **`Monster` (적 클래스)**
* **상속 및 팩토리 메서드**: `Character`를 상속받고 처치 보상 경험치(`exp_reward`) 속성 추가
* **랜덤 생성**: `@classmethod` 방식의 `create_random()`을 구현해 난이도에 맞는 몬스터를 무작위 생성

### AI 결과 검증 (Verification Checklist)

AI가 만든 game loop code를 아래 기준으로 검증한다. 
* [ ] **1. 캐릭터 생성 검증**: `Warrior`, `Wizard`, `Healer` 객체 생성 시 직업별 초기 스탯(HP, 공격력, 방어력, 마나/분노)이 올바르게 초기화되고 `__str__` 문자열 출력이 정상 작동하는지 확인
* [ ] **2. 일반 공격 검증**: `attack()` 호출 시 방어력이 반영된 데미지 계산(`공격력 - 방어력`)과 피격 대상 HP 차감, 직업별 고유 자원 변화(전사 분노 +15, 마법사 마나 -10)가 정상 작동하는지 확인
* [ ] **3. 특수 능력 검증**: 자원 조건 충족 후 `special()` 호출 시 직업별 다형성 스킬(전사: 분노 폭발 2배 피해, 마법사: 마나 소모 3배 파이어볼, 힐러: 마나 소모 자가 회복)이 정상 동작하는지 확인
* [ ] **4. 몬스터 생성 검증**: `Monster.create_random(difficulty=2)` 실행 시 지정된 난이도 범위 내의 몬스터 데이터가 무작위로 튜플 언패킹되어 정상 생성되는지 확인
* [ ] **5. 성장 시스템 검증**: `gain_exp(250)` 호출 시 경험치가 누적되어 레벨업 조건(`Lv * 100`)을 달성하고, 연쇄 레벨업(Lv.1 → Lv.3) 및 스탯 상승/최대 HP 회복이 정상 처리되는지 확인
* [ ] **6. 클래스 관리 검증**: `Character.get_total_count()` 호출 시 생성된 전체 캐릭터 및 몬스터 인스턴스 수(`total_count`)가 정확하게 누적 카운트되는지 확인

#### 입력 처리

- [ ] 잘못된 입력 시 exception handling이 되는가? (문자, 범위 밖 숫자 등)
- [ ] 잘못된 입력 시 turn을 낭비하지 않고 다시 입력받는가?

#### class 활용

- [ ] 직접 만든 class code를 수정 없이 사용하고 있는가? 
- [ ] 각 직업의 `attack()` , `special()`이 다르게 동작하는가? (polymorphism)
- [ ] `Monster.create_random()`을 사용하여 Monster를 생성하는가?

#### 출력

- [ ] 전투 상황이 읽기 쉽게 출력하는가?
- [ ] Player와 Monster의 상태(`__str__`)가 매 turn 표시되는가?

---

### Test

1. 각 직업(Warrior, Wizard, Healer)으로 한 번씩 Play해 보자.
2. 잘못된 입력을 넣어 보자. (숫자 대신 문자, 범위 밖 숫자 등)
3. AI가 만든 code에서 문제가 발견되면?
    - 문제를 정확히 설명해서 AI에게 수정을 요청한다.
    - 예: "Healer의 special()에서 자기 자신을 치유해야 하는데 Monster를 치유하고 있어. 수정해 줘."
4. 직접 수정할 수 있는 부분은 직접 수정해 보자.

---
### 추가 검증 체크리스트
- [ ] 1. 기존 코드 호환성 검증: AI 코드 반영 후에도 before_ai_test()가 오류 없이 실행되며, 기존 캐릭터/직업/몬스터 동작 및 total_count 집계가 정상 유지되는지 확인

- [ ] 2. 아이템 다형성(Polymorphism) 검증: Item 클래스를 상속받은 4종 아이템(HealthPotion, ManaPotion, AttackElixir, Bomb)의 use() 메서드가 각 직업 및 대상에 맞춰 개별 효과로 정상 오버라이딩되어 실행되는지 확인

- [ ] 3. 인벤토리 및 예외 처리 검증: 아이템 사용 시 인벤토리 목록에서 정상 차감되는지, 마나가 없는 전사가 마나 포션을 사용하거나 빈 인벤토리를 조회할 때 예외 안내 문구가 출력되는지 확인

- [ ] 4. 전리품 드롭 시스템 검증: 몬스터 처치 이벤트 발생 시 getRandomItem()을 통해 무작위 아이템이 인벤토리에 정상 추가되는지 확인

- [ ] 5. 전투 루프 통합 검증: 탐색 → 몬스터 조우 → 턴제 전투(일반/특수/아이템) → 승리/패배 및 최종 결과 출력까지의 게임 진행 흐름이 매끄럽게 연결되는지 확인

## 📖 원본 코드와 수업 메모

### 프로젝트 설계 주석과 테스트 점검표

원본 Python 파일 앞부분의 구현 요구사항 및 단위 테스트 체크리스트이다. 수업에서 작성한 주석을 보존했다.

```python

# 프로젝트 1: 텍스트 RPG 전투 게임

# 핵심 class 구현

# 주요 내용

    # class Character에서 ====================
    # 모든 Character(Monster 포함)의 parents class
    # 공통 속성:
        # name: 이름, public - 외부(child class 포함)에서 자유롭게 접근
        # __HP: 현재 HP, private - property로만 접근 가능(읽기, 쓰기)
        # __max_hp: 최대 HP, private - 레벨업 시에 증가 시킴
        # __attack_power: 공격력, private - 읽기 전용
        # __defense: 방어력, private - 읽기 전용
        # __exp: 경험치, private - gain_exp()로만 증가
        # __level: 레벨, private - 읽기 전용, 레벨업 시 자동증가
        # Character.total_count: Character 생성 시 +1

    # 출력 관련

        # __str__(self)
        # __repr__(self)

    # 상태 정보

        # is_alive(self): 생존 여부, property로 읽기 전용

    # 공통 기능:
        # take_damage(self, damage): 피격, 실제 적용 damage return(현재 필요 없음)
        # attack(self, other): 공격, 주로 child character에서 override
        # special(self): 특수 능력, 주로 child character에서 override
        # gain_exp(self, amount): 경험 획득(레벨업 판단하여 실행)
        # __level_up(self): 레벨업 class, name mangling으로 현재 class에서만 접근 가능(현재 class의 gain_exp()에서만 접근 가능하게 한다.)

    # classmethod:
        # get_total_count(cls):
    # staticmethod:
        # calculate_damage(attack, defense): 공격량과 방어량을 계산해서 실제 적용되는 damage를 return하는 utility


### 📋 클래스 단위 테스트 점검표 (Checklist)

#### 1. 기본 메커니즘 및 캡슐화 테스트

# * [ ] **최대 HP 제한 검증**
# * `hp` 변수에 `max_hp`보다 큰 값을 대입할 때 `max_hp`로 상한이 유지되는지 확인


# * [ ] **최소 HP 제한 검증**
# * `hp` 변수에 0 미만의 음수 값을 대입할 때 0으로 하한이 유지되는지 확인


# * [ ] **생존 상태 확인 (`is_alive`)**
# * `hp`가 0일 때 `is_alive` 속성이 `False`를 반환하고, 1 이상일 때 `True`를 반환하는지 확인


# * [ ] **최소 데미지 보장 계산**
# * 피격자의 방어력이 공격력보다 높은 경우에도 데미지가 0 이하로 떨어지지 않고 최소 `1`로 처리되는지 확인


# * [ ] **캐릭터 생성 카운트 관리**
# * 객체가 생성될 때마다 `Character.total_count`가 정상적으로 1씩 증가하는지 확인



#### 2. 직업별 특수 동작 및 마나/분노 시스템 테스트

# * [ ] **전사 (Warrior) 특수기 제어**
# * 분노 수치가 조건(30 이상) 미만일 때 `special()` 사용이 거부되고, 조건 충족 시 정상 발동하는지 확인


# * [ ] **마법사 (Wizard) 마나 고갈 및 공격 전환**
# * 일반 공격 시 마나가 10씩 정상 차감되는지 확인
# * 마나가 10 미만일 때 마법 공격 대신 기본 지팡이 공격(데미지 5)으로 전환되는지 확인


# * [ ] **마법사 (Wizard) 특수기 마나 소모**
# * 파이어볼(`special()`) 사용 시 마나 40 소모 및 3배 데미지 적용 여부 확인


# * [ ] **힐러 (Healer) 타겟 치유**
# * 대상 지정(`target`) 시 해당 캐릭터의 HP가 회복되고, 회복 시 `hp` setter의 유효성 검사를 거치는지 확인


# * [ ] **힐러 (Healer) 자가 치유**
# * 대상을 지정하지 않을 경우(`target=None`) 자기 자신을 대상으로 치유 스킬이 작동하는지 확인



# #### 3. 레벨업 및 성장 시스템 테스트

# * [ ] **경험치 누적 및 레벨업 조건**
# * 경험치(`exp`) 획득 시 `현재 레벨 × 100` 조건 충족에 따라 레벨업이 발생하며 경험치가 누적되는지 확인


# * [ ] **연속 레벨업 처리**
# * 한 번에 다량의 경험치를 얻었을 때 `while` 루프를 통해 연속으로 레벨업이 진행되는지 확인


# * [ ] **레벨업 능력치 보상 및 회복**
# * 레벨업 시 스탯(HP 상한, 공격력, 방어력) 증가 적용 및 HP가 최대치(`max_hp`)로 전액 회복되는지 확인



# #### 4. 몬스터 시스템 테스트

# * [ ] **몬스터 전용 데이터 관리**
# * 몬스터 객체 생성 시 보상 경험치(`exp_reward`) 속성이 정상 설정되고 호출되는지 확인


# * [ ] **랜덤 몬스터 생성 (`create_random`)**
# * `difficulty` 파라미터 전달 시 해당 범위(슬라이싱) 내의 몬스터 목록 중 1종이 정상 무작위 생성되는지 확인
```

### 아이템 클래스와 다형성

`Item.use()`를 자식 클래스가 재정의한다. 체력·마나 포션, 공격력 영약, 폭탄이 서로 다른 기능을 제공한다.

```python
import random
import time


# --- Item 클래스 다형성 구조 구현 ---
class Item:

    def __init__(self, name, description):
        self.name = name  # public
        self.description = description  # public

    def use(self, user, target=None):
        """서브클래스에서 각 아이템 효과를 다형성(Polymorphism)으로 오버라이딩"""
        raise NotImplementedError("서브클래스에서 구현해야 합니다.")

    def __str__(self):
        return f"[{self.name}] {self.description}"


class HealthPotion(Item):

    def __init__(self, heal_amount=50):
        super().__init__("체력 포션", f"HP를 {heal_amount} 회복합니다.")
        self.heal_amount = heal_amount

    def use(self, user, target=None):
        target_char = target if target else user
        target_char.hp += self.heal_amount
        print(
            f"🧪 {self.name} 사용! {target_char.name}의 HP가"
            f" +{self.heal_amount} 회복되었습니다. (현재 HP: {target_char.hp})"
        )


class ManaPotion(Item):

    def __init__(self, mana_amount=40):
        super().__init__("마나 포션", f"마나를 {mana_amount} 회복합니다.")
        self.mana_amount = mana_amount

    def use(self, user, target=None):
        if hasattr(user, "restore_mana"):
            user.restore_mana(self.mana_amount)
            print(
                f"🧪 {self.name} 사용! {user.name}의 마나가"
                f" +{self.mana_amount} 회복되었습니다."
            )
        else:
            print(
                f"⚠️ {user.name}은(는) 마나가 없는 직업입니다! (아이템만"
                " 소모됨)"
            )


class AttackElixir(Item):

    def __init__(self, buff_amount=10):
        super().__init__(
            "영웅의 영약", f"공격력을 영구적으로 +{buff_amount} 상승시킵니다."
        )
        self.buff_amount = buff_amount

    def use(self, user, target=None):
        user.boost_attack_power(self.buff_amount)
        print(
            f"🍷 {self.name} 복용! {user.name}의 영구 공격력이"
            f" +{self.buff_amount} 상승했습니다! (현재 공격력:"
            f" {user.attack_power})"
        )


class Bomb(Item):

    def __init__(self, damage=45):
        super().__init__(
            "화염 폭탄",
            f"적에게 방어력을 무시하는 {damage}의 고정 데미지를 입힙니다.",
        )
        self.damage = damage

    def use(self, user, target=None):
        if target and target.is_alive:
            target.hp -= self.damage
            print(
                f"💣 {self.name} 투척! {target.name}에게 {self.damage}의"
                f" 방어 무시 데미지! (남은 HP: {target.hp})"
            )
        else:
            print("⚠️ 대상이 없거나 이미 처치된 적입니다!")
```

### Character 기본 클래스

HP 상한 관리, 읽기 전용 속성, 데미지 처리, 경험치와 레벨업, 클래스·정적 메서드가 구현되어 있다.

```python
# --- Character 기본 클래스 ---
class Character:

    total_count = 0

    def __init__(self, name, hp, attack_power, defense):
        self.name = name  # public
        self.__max_hp = hp  # private - 최대 hp
        self.__hp = hp  # private
        self.__attack_power = attack_power  # private - 공격력
        self.__defense = defense  # private - 방어력
        self.__exp = 0  # private - 경험치 값
        self.__level = 1  # private - 레벨
        Character.total_count += 1  # Character 수 증가

    # -- 출력 관련 --
    def __str__(self):
        return (
            f"[Lv.{self.__level}] {self.name} (HP:"
            f" {self.__hp}/{self.__max_hp})"
        )

    def __repr__(self):
        return (
            f"Character('{self.name}', {self.__hp}, {self.__attack_power},"
            f" {self.__defense})"
        )

    # --- property: 외부에서 변수처럼 접근 ---
    @property
    def hp(self):
        return self.__hp

    @hp.setter
    def hp(self, value):
        if value < 0:
            self.__hp = 0
        elif value > self.__max_hp:
            self.__hp = self.__max_hp
        else:
            self.__hp = value

    @property
    def attack_power(self):
        return self.__attack_power

    @property
    def defense(self):
        return self.__defense

    @property
    def level(self):
        return self.__level

    @property
    def is_alive(self):
        return self.__hp > 0

    def boost_attack_power(self, amount):
        """아이템 효과 등을 통한 공격력 영구 상승 메서드"""
        self.__attack_power += amount

    # --- class method / static method ---
    @classmethod
    def get_total_count(cls):
        return cls.total_count

    @staticmethod
    def calculate_damage(attack, defense):
        damage = attack - defense
        if damage < 1:
            damage = 1
        return damage

    # --- 전투 관련 method ---
    def take_damage(self, damage):
        actual_damage = Character.calculate_damage(damage, self.__defense)
        self.hp = self.__hp - actual_damage
        print(
            f"{self.name}이 {actual_damage}의 데미지를 받았다! (남은 HP:"
            f" {self.__hp})"
        )
        return actual_damage

    def attack(self, other):
        print(f"⚔️ {self.name}이 {other.name}을 공격!")
        other.take_damage(self.__attack_power)

    def special(self, other=None):
        return f"{self.name}: 특수 능력 없음"

    # --- 성장 관련 method ---
    def gain_exp(self, amount):
        self.__exp += amount
        print(f"{self.name}이 경험치 {amount}를 획득! (총 경험치: {self.__exp})")

        while self.__exp >= self.level * 100:
            self.__level_up()

    def __level_up(self):
        self.__level += 1
        self.__max_hp += 20
        self.__hp = self.__max_hp
        self.__attack_power += 5
        self.__defense += 3
        print(f"🎉 레벨 업! {self.name} → Lv.{self.__level}!")
```

### 직업별 클래스: Warrior, Wizard, Healer

전사는 분노, 마법사는 마나, 힐러는 회복 능력을 활용한다. 각 직업이 `attack()`과 `special()`을 재정의한다.

```python
# --- 직업 class 구현 ---


class Warrior(Character):

    def __init__(self, name):
        super().__init__(name, hp=150, attack_power=25, defense=15)
        self.__rage = 0  # private - 분노 게이지

    def attack(self, other):
        print(f"⚔️ {self.name}이 {other.name}에게 강력한 검 베기!")
        self.__rage += 15
        other.take_damage(self.attack_power * 2)

    def special(self, other):
        if self.__rage >= 30:
            print(f"🔥 {self.name}의 분노 폭발!")
            self.__rage = 0
            other.take_damage(self.attack_power * 2)
        else:
            print(f"분노가 부족합니다. (현재: {self.__rage}/30)")

    def __str__(self):
        return (
            f"[Lv.{self.level}] ⚔️ 전사 {self.name} (HP: {self.hp}) [분노:"
            f" {self.__rage}] "
        )


class Wizard(Character):

    def __init__(self, name):
        super().__init__(name, hp=80, attack_power=35, defense=8)
        self.__max_mana = 100
        self.__mana = 100  # private - 마나

    def restore_mana(self, amount):
        self.__mana = min(self.__max_mana, self.__mana + amount)

    def attack(self, other):
        if self.__mana >= 10:
            print(f"\n🔮 {self.name}이 {other.name}에게 마법 화살 발사!")
            self.__mana -= 10
            other.take_damage(self.attack_power)
        else:
            print(
                f"\n👊 {self.name}이 {other.name}을 지팡이로 때린다!"
                " (마나 부족)"
            )
            other.take_damage(5)

    def special(self, other):
        if self.__mana >= 40:
            print(f"\n🧙 {self.name}의 파이어볼!")
            self.__mana -= 40
            other.take_damage(self.attack_power * 3)
        else:
            print(f"마나가 부족합니다! (현재: {self.__mana}/40)")

    def __str__(self):
        return (
            f"[Lv.{self.level}] 🧙 마법사 {self.name} (HP: {self.hp}) [마나:"
            f" {self.__mana}/{self.__max_mana}]"
        )


class Healer(Character):

    def __init__(self, name):
        super().__init__(name, hp=100, attack_power=15, defense=10)
        self.__max_mana = 80
        self.__mana = 80  # private - 마나

    def restore_mana(self, amount):
        self.__mana = min(self.__max_mana, self.__mana + amount)

    def attack(self, other):
        print(f"\n✨ {self.name}이 {other.name}에게 빛의 화살!")
        other.take_damage(self.attack_power)

    def special(self, target=None):
        if self.__mana >= 30:
            if target is None:
                target = self
            heal_amount = 40
            print(
                f"\n💚 {self.name}을(를) 치유! {target.name}의 HP +"
                f" {heal_amount}"
            )
            self.__mana -= 30
            target.hp += heal_amount
            print(f"{target.name}의 현재 HP: {target.hp}")
        else:
            print(f"마나가 부족합니다! (현재: {self.__mana}/30)")

    def __str__(self):
        return (
            f"[Lv.{self.level}] 💚 힐러 {self.name} (HP: {self.hp}) [마나:"
            f" {self.__mana}/{self.__max_mana}]"
        )
```

### Monster 클래스와 랜덤 생성

`Monster.create_random()`은 난이도에 맞춰 몬스터 정보를 선정해 객체를 생성한다.

```python
# --- Monster class 구현 ---


class Monster(Character):

    monster_count = 0

    def __init__(self, name, hp, attack_power, defense, exp_reward):
        super().__init__(name, hp, attack_power, defense)
        self.__exp_reward = exp_reward
        Monster.monster_count += 1

    @property
    def exp_reward(self):
        return self.__exp_reward

    def __str__(self):
        return f"👾 {self.name} (HP: {self.hp})"

    @classmethod
    def create_random(cls, difficulty=2):
        monsters = [
            ("슬라임", 40, 20, 3, 30),
            ("고블린", 70, 30, 5, 50),
            ("오크", 120, 40, 12, 80),
            ("드래곤", 200, 55, 25, 200),
        ]
        available = monsters[: min(difficulty, len(monsters))]
        data = random.choice(available)
        return cls(*data)
```

### 기존 동작 테스트

`before_ai_test()`에 클래스별 공격·특수 능력·레벨업·객체 수 확인 과정이 남아 있다.

```python
# --- 기존 동작 테스트 유지 ---
def before_ai_test():
    warrior = Warrior("Arthur")
    wizard = Wizard("Merlin")
    healer = Healer("Aria")

    print(warrior)
    print(wizard)
    print(healer)

    warrior.attack(wizard)
    wizard.attack(warrior)
    healer.attack(warrior)

    warrior.attack(wizard)
    warrior.attack(wizard)
    warrior.special(wizard)
    wizard.special(warrior)
    healer.special()

    monster = Monster.create_random(difficulty=2)
    print(monster)

    warrior.gain_exp(250)

    print(f"총 Character 수: {Character.get_total_count()}")
```

### 로딩 출력과 아이템 드롭

로딩 출력 함수와 무작위 아이템 획득 함수가 게임 진행을 보조한다.

```python
# Game Loop
def loading_text(text="⚔️ 전투 진행 중", delay=1.0):
    print(text, end="", flush=True)
    for _ in range(3):
        time.sleep(delay / 3)
        print(".", end="", flush=True)
    print("\n")


def getRandomItem():
    """몬스터 처치 시 무작위 드롭 아이템 생성"""
    items = [
        HealthPotion(50),
        ManaPotion(40),
        AttackElixir(8),
        Bomb(50),
    ]
    return random.choice(items)
```

### 전투 루프: run_game()

직업 선택, 몬스터 조우, 턴제 공격·특수 능력·아이템 사용, 경험치 획득, 승패 판단으로 연결되는 메인 진행 코드이다.

```python
def run_game():
    print("=" * 50)
    print("⚔️ TEXT RPG BATTLE GAME ⚔️")
    print("=" * 50)

    player_name = input("플레이어의 이름을 입력하세요: ").strip() or "용사"

    print("\n[직업 선택]")
    print("1. 전사 (Warrior)")
    print("2. 마법사 (Wizard)")
    print("3. 힐러 (Healer)")

    while True:
        job_choice = input("선택 (1~3): ").strip()
        if job_choice == "1":
            player = Warrior(player_name)
            break
        elif job_choice == "2":
            player = Wizard(player_name)
            break
        elif job_choice == "3":
            player = Healer(player_name)
            break
        print("올바른 번호를 입력해 주세요.")

    # 시작 시 다양한 소비 아이템 인벤토리 구성
    inventory = [HealthPotion(50), ManaPotion(40), AttackElixir(10), Bomb(45)]
    monsters_defeated = 0
    target_monsters = 3

    print(f"\n🎮 게임 시작! [{player}]")

    while monsters_defeated < target_monsters and player.is_alive:
        loading_text("\n🔍 주변을 탐색하는 중", 1.2)

        current_monster = Monster.create_random(difficulty=monsters_defeated + 1)
        print("=" * 40)
        print(
            f"👾 야생의 {current_monster.name}이(가) 나타났습니다!"
            f" ({monsters_defeated + 1}/{target_monsters})"
        )
        print("=" * 40)

        while current_monster.is_alive and player.is_alive:
            print(
                f"\n--- [상태] {player} | 🎒 아이템 수량: {len(inventory)}개"
                " ---"
            )
            print(f"--- [적]   {current_monster} ---")

            print("\n[플레이어 턴]")
            print("1. 일반 공격  2. 특수 공격  3. 인벤토리 (소비 아이템)")
            action = input("행동 선택 (1~3): ").strip()

            if action == "1":
                loading_text("⚔️ 공격 진행 중", 0.8)
                player.attack(current_monster)

            elif action == "2":
                loading_text("✨ 특수 기술 준비 중", 0.8)
                if isinstance(player, Healer):
                    player.special()
                else:
                    player.special(current_monster)

            elif action == "3":
                if not inventory:
                    print("🎒 인벤토리가 비어있습니다!")
                    continue

                print("\n[🎒 인벤토리 목록]")
                for idx, item in enumerate(inventory, 1):
                    print(f" {idx}. {item}")
                print(" 0. 취소 (턴 유지)")

                item_choice = input("사용할 아이템 번호 선택: ").strip()
                if item_choice.isdigit() and 1 <= int(item_choice) <= len(
                    inventory
                ):
                    selected_item = inventory.pop(int(item_choice) - 1)
                    loading_text(
                        f"🧪 {selected_item.name} 사용 중", 0.8
                    )

                    # 아이템 사용 대상 구분 (폭탄은 적 대상, 나머지는 플레이어 대상)
                    if isinstance(selected_item, Bomb):
                        selected_item.use(player, current_monster)
                    else:
                        selected_item.use(player)
                else:
                    print("아이템 사용을 취소했습니다.")
                    continue
            else:
                print("잘못된 입력입니다. 턴을 넘어갑니다.")

            time.sleep(0.8)

            # 몬스터 처치 체크
            if not current_monster.is_alive:
                loading_text("\n💥 최후의 일격", 1.0)
                print(f"🎉 {current_monster.name}을(를) 처치했습니다!")
                monsters_defeated += 1
                player.gain_exp(current_monster.exp_reward)

                # 아이템 드롭 전리품
                dropped_item = getRandomItem()
                inventory.append(dropped_item)
                print(
                    f"🎁 전리품 획득! [{dropped_item.name}]을(를)"
                    " 인벤토리에 얻었습니다."
                )

                time.sleep(1.0)
                break

            # 몬스터 반격 턴
            loading_text(f"\n👾 {current_monster.name}의 반격 준비 중", 0.9)
            current_monster.attack(player)
            time.sleep(0.8)

    print("\n" + "=" * 50)
    print("🏁 게임 종료 🏁")
    print("=" * 50)

    if player.is_alive and monsters_defeated >= target_monsters:
        print(
            f"🏆 승리! {player.name}이(가) 모든"
            f" 몬스터({target_monsters}마리)를 처치했습니다!"
        )
    else:
        print(f"💀 패배... {player.name}이(가) 전투 중 사망했습니다.")

    print("\n[최종 게임 결과]")
    print(f"- 플레이어 상태 : {player}")
    print(f"- 처치한 몬스터 : {monsters_defeated} / {target_monsters} 마리")
    print(f"- 남은 아이템 수 : {len(inventory)} 개")
```

### 프로그램 실행 진입점

직접 실행할 때 `run_game()`을 호출한다.

```python
if __name__ == "__main__":
    run_game()
```

## ✅ 핵심 정리 및 확인 사항

원본 코드는 수정하지 않았다. 아래 항목은 읽거나 실행할 때 확인하면 좋은 부분이다.

- 업로드된 별도 `07_project_ai_vibe_coding.md`의 목표·게임 흐름·검증 체크리스트를 앞부분에 병합했다. 체크박스의 Markdown 문법만 올바르게 정리했다.
- 현재 `run_game()`에서는 1~3 이외의 행동을 입력하면 “턴을 넘어갑니다”라고 출력한 뒤 몬스터가 반격한다. 제공된 체크리스트의 “잘못된 입력이 턴을 소모하지 않아야 한다” 기준과 다르므로 보완 대상이다.
- `before_ai_test()`는 정의되어 있지만 `if __name__ == "__main__":`에서 호출되지 않는다. 별도 호출해야 테스트가 실행된다.
- 프로젝트 전체 코드는 하나의 Python 파일을 위에서 아래로 읽은 순서대로 구간별로 나눠 실었다. 각 코드 블록만 단독 실행하면 앞에서 정의한 클래스가 없어 오류가 발생할 수 있다.
