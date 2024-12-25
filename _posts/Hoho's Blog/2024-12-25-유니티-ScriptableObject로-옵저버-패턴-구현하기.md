---
title: 유니티 ScriptableObject로 옵저버 패턴 구현하기
date: 2024-12-25 21:58:08 +09:00
categories:
  - 유니티
  - 디자인패턴
tags:
  - ScriptableObject
  - 디자인패턴
  - 유니티
  - 옵저버패턴
---

옵저버 패턴은 **객체 간 의존성을 최소화**하면서 이벤트 기반 시스템을 설계할 수 있는 유용한 디자인 패턴입니다. 유니티에서 ScriptableObject를 사용하면 이러한 패턴을 손쉽게 구현할 수 있습니다. 이 글에서는 ScriptableObject를 활용한 옵저버 패턴 구현 방법과 코드 예제를 제공합니다.

---

# 1. ScriptableObject란?

ScriptableObject는 유니티에서 제공하는 데이터 저장 및 공유를 위한 클래스입니다.

- 메모리 사용량이 적고, 에디터에서 쉽게 관리할 수 있습니다.
    
- 게임 오브젝트에 종속되지 않으므로, 전역 데이터 관리에 유용합니다.
    

---

# 2. 옵저버 패턴 개념

옵저버 패턴은 하나의 객체(주제)가 변경될 때 이를 관찰하고 있는 다른 객체(옵저버)들에게 알림을 전달하는 패턴입니다.

- **주제(Subject)**: 상태를 보유하며, 옵저버를 관리합니다.
    
- **옵저버(Observer)**: 주제를 관찰하며, 상태 변화에 반응합니다.
    

유니티에서는 이 패턴을 이벤트와 델리게이트로 구현할 수 있지만, ScriptableObject를 사용하면 더 유연하고 직관적인 구조를 설계할 수 있습니다.

---

# 3. ScriptableObject 기반 옵저버 패턴 구현

## (1) 이벤트 시스템 정의

```
using UnityEngine;
using System;

[CreateAssetMenu(fileName = "GameEvent", menuName = "Events/GameEvent")]
public class GameEvent : ScriptableObject
{
    private event Action listeners;

    public void RaiseEvent()
    {
        if (listeners != null)
            listeners.Invoke();
    }

    public void RegisterListener(Action listener)
    {
        listeners += listener;
    }

    public void UnregisterListener(Action listener)
    {
        listeners -= listener;
    }
}
```

이 코드는 ScriptableObject를 활용하여 이벤트를 관리합니다.

- `RaiseEvent`: 이벤트를 발생시킵니다.
    
- `RegisterListener`: 이벤트에 리스너를 등록합니다.
    
- `UnregisterListener`: 이벤트에서 리스너를 제거합니다.
    

## (2) 옵저버 스크립트 작성

```
using UnityEngine;

public class EventListener : MonoBehaviour
{
    public GameEvent gameEvent;

    private void OnEnable()
    {
        gameEvent.RegisterListener(OnEventRaised);
    }

    private void OnDisable()
    {
        gameEvent.UnregisterListener(OnEventRaised);
    }

    private void OnEventRaised()
    {
        Debug.Log($"{gameEvent.name} 이벤트 발생!");
    }
}
```

옵저버는 GameEvent를 관찰하며, 이벤트가 발생하면 `OnEventRaised` 메서드를 호출합니다.

## (3) 주제 스크립트 작성

```
using UnityEngine;

public class EventTrigger : MonoBehaviour
{
    public GameEvent gameEvent;

    public void TriggerEvent()
    {
        gameEvent.RaiseEvent();
    }
}
```

이 스크립트는 특정 시점에 이벤트를 발생시키는 역할을 합니다. 예를 들어, 버튼 클릭 이벤트와 연결할 수 있습니다.

---

# 4. 에디터 설정

1. `GameEvent` ScriptableObject 생성: 프로젝트 창에서 우클릭 > Create > Events > GameEvent.
    
2. 이벤트 트리거와 리스너 설정:
    
    - `EventTrigger` 스크립트를 가진 오브젝트에 `GameEvent`를 연결.
        
    - `EventListener` 스크립트를 가진 오브젝트에 같은 `GameEvent`를 연결.
        

---

# 5. 활용 사례

- **UI 시스템**: 버튼 클릭 시 여러 UI 업데이트 처리.
    
- **게임 이벤트**: 적이 사망하면 점수 증가, 애니메이션 트리거 등.
    
- **전역 알림**: 게임 상태 변경을 전역적으로 알림.
    

---

# 6. 마무리

ScriptableObject를 활용한 옵저버 패턴은 간결하고 재사용성이 높은 코드 구조를 제공합니다. 이를 통해 유니티 프로젝트에서 이벤트 기반 시스템을 효과적으로 설계해 보세요. 필요에 따라 로직을 확장하거나 커스터마이징할 수도 있습니다.

질문이나 피드백이 있다면 댓글로 남겨주세요!