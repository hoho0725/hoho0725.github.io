---
title: Unity에서 ScriptableObject를 활용한 전략 패턴 구현
date: 2024-12-23 04:32:36 +09:00
categories:
  - 유니티
  - 디자인패턴
tags:
  - 유니티
  - 디자인패턴
  - 전략패턴
  - ScriptableObject
---


# Unity에서 ScriptableObject를 활용한 전략 패턴 구현

Unity 프로젝트에서 **전략 패턴(Strategy Pattern)** 을 구현하면 특정 행동이나 알고리즘을 동적으로 변경할 수 있습니다. 특히 Unity의 **ScriptableObject**를 활용하면 더욱 간결하고 직관적인 방식으로 이 패턴을 적용할 수 있습니다. 이 글에서는 **플레이어 공격 방식**을 예로 들어, 전략 패턴을 ScriptableObject와 함께 구현하는 방법을 단계별로 설명합니다.

---

## 1. 전략 패턴이란?

전략 패턴은 **행동을 정의하는 알고리즘을 각각 독립적으로 캡슐화**하고, 이를 필요에 따라 동적으로 교체할 수 있도록 설계하는 디자인 패턴입니다. 이 패턴에서는 인터페이스나 클래스를 통해 공통된 행동을 정의하고, 이를 상속받은 구체적인 클래스가 각각의 세부적인 행동을 구현합니다. Unity에서는 이를 통해 캐릭터의 이동, 공격, AI 행동 등을 유연하게 설계할 수 있습니다.

---

## 2. ScriptableObject란?

**ScriptableObject**는 Unity에서 제공하는 데이터 중심 설계 패턴입니다. 게임 내 데이터를 에디터에서 쉽게 관리할 수 있으며, 오브젝트 간 상태를 공유하거나 다양한 설정을 적용하는 데 유용합니다.

---

## 3. ScriptableObject를 활용한 전략 패턴 구현

플레이어의 **공격 방식**을 동적으로 교체하는 예제를 통해 구현 방법을 살펴보겠습니다.

### 3.1 인터페이스 정의

먼저, 모든 공격 전략이 따라야 할 인터페이스를 정의합니다.

```C#
public interface IAttackStrategy
{
    void Attack(GameObject attacker);
}
```

이 인터페이스는 모든 공격 전략이 구현해야 하는 `ExecuteAttack` 메서드를 포함합니다.

---

### 3.2 ScriptableObject로 전략 구현

다양한 공격 전략을 ScriptableObject를 통해 구현합니다.다양한 공격 전략을 ScriptableObject를 통해 구현합니다. 
#### ScriptableObject로 기본 공격전략 구현

```C#
public abstract class AttackStrategy : ScriptableObject, IAttackStrategy
{ 
    public abstract void Attack(GameObject attacker); 
}
```

#### 근접 공격 전략

```C#
using UnityEngine;

[CreateAssetMenu(menuName = "Strategies/MeleeAttack")]
public class MeleeAttackStrategy : AttackStrategy
{
    public void Attack(GameObject attacker)
    {
        Debug.Log("Melee Attack!");
        // 근접 공격 로직
    }
}
```

#### 원거리 공격 전략

```C#
using UnityEngine;

[CreateAssetMenu(menuName = "Strategies/RangedAttack")]
public class RangedAttackStrategy : AttackStrategy
{
    public void Attack(GameObject attacker)
    {
        Debug.Log("Ranged Attack!");
        // 원거리 공격 로직
    }
}
```

---

### 3.3 컨텍스트 클래스 (캐릭터)

`Character` 클래스는 현재 사용 중인 공격 전략을 보유하고 이를 실행하는 역할을 합니다. ScriptableObject를 사용하였기 때문에 인스펙터창에서 바로 설정할 수 있고 디버깅도 쉽습니다.

```C#
using UnityEngine;

public class Character : MonoBehaviour
{
    public AttackStrategy attackStrategy; // 현재 공격 전략

    public void PerformAttack()
    {
        if (strategy != null)
        {
            attackStrategy.ExecuteAttack(gameObject);
        }
        else
        {
            Debug.LogWarning("No attack strategy assigned!");
        }
    }

    public void SetAttackStrategy(attackStrategy newStrategy)
    {
        attackStrategy = newStrategy;
    }
}
```

---

### 3.4 전략 변경 및 테스트

#### 전략 변경 스크립트

```C#
using UnityEngine;

public class StrategyTester : MonoBehaviour
{
    public Character character;
    public AttackStrategy meleeStrategy;
    public AttackStrategy rangedStrategy;

    void Update()
    {
        if (Input.GetKeyDown(KeyCode.Alpha1))
        {
            character.SetAttackStrategy(meleeStrategy);
        }
        else if (Input.GetKeyDown(KeyCode.Alpha2))
        {
            character.SetAttackStrategy(rangedStrategy);
        }

        if (Input.GetKeyDown(KeyCode.Space))
        {
            character.PerformAttack();
        }
    }
}
```

#### Unity 설정

1. `Assets > Create > Strategies`에서 **MeleeAttack** 및 **RangedAttack** ScriptableObject를 생성합니다.
    
2. `Character` GameObject에 `Character` 스크립트를 추가하고 `AttackStrategy` 필드에 ScriptableObject를 연결합니다.
    
3. `StrategyTester` 스크립트를 사용해 전략을 동적으로 변경하고 테스트합니다.
    

---

## 4. 실행 결과

1. **KeyCode.Alpha1**: 근접 공격 전략으로 변경
    
2. **KeyCode.Alpha2**: 원거리 공격 전략으로 변경
    
3. **Space**: 현재 선택된 전략에 따라 공격 실행
    

### 결과 로그 예시

- "Melee Attack!"
    
- "Ranged Attack!"
    

---

## 5. 확장 가능성

이 구조를 사용하면 다양한 공격 전략을 쉽게 추가할 수 있습니다. 예를 들어:

- **마법 공격** 전략
    
- **폭발 공격** 전략
    

각 전략은 ScriptableObject로 구현하면 됩니다.

---

## 6. 전략패턴을 ScriptableObject로 구현할 시 장점

**ScriptableObject**를 사용하면 Monobehaviour와 달리 동일한 인스턴스를 사용하기에 메모리 효율성이 증가합니다. 또한 독립적으로 존재하기에 디버깅하기에도 용이합니다.

---
## 6. 결론

Unity에서 ScriptableObject와 전략 패턴을 결합하면 데이터 중심의 설계를 통해 유연하고 확장 가능한 시스템을 구현할 수 있습니다. 이 접근 방식은 특히 캐릭터 행동, AI, 게임 규칙 등 다양한 상황에서 효과적으로 사용할 수 있습니다.