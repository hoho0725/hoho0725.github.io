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

## 1. ScriptableObject란?

ScriptableObject의 개념과 장점에 대해서는 **[이 글]({{ site.baseurl }}/posts/Unity-ScriptableObject-%ED%9A%A8%EC%9C%A8%EC%A0%81%EC%9D%B8-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EA%B4%80%EB%A6%AC-%EB%B0%A9%EB%B2%95/)을** 참조하세요. 이를 통해 ScriptableObject가 무엇인지 더 깊이 이해할 수 있습니다.

> [!info] info
> The benefit of reading the author information from the file `_data/authors.yml`{: .filepath } is that the page will have the meta tag `twitter:creator`, which enriches the [Twitter Cards](https://developer.twitter.com/en/docs/twitter-for-websites/cards/guides/getting-started#card-and-content-attribution) and is good for SEO.
{: .prompt-info }






---

## 2. 옵저버 패턴 개념

옵저버 패턴은 하나의 **객체(주제)가** 변경될 때 이를 관찰하고 있는 다른 **객체(옵저버)들**에게 알림을 전달하는 패턴입니다.

- **주제(Subject)**: 상태를 보유하며, 옵저버를 관리합니다.
    
- **옵저버(Observer)**: 주제를 관찰하며, 상태 변화에 반응합니다.
    

유니티에서는 이 패턴을 이벤트와 델리게이트로 구현할 수 있지만, ScriptableObject를 사용하면 더 유연하고 직관적인 구조를 설계할 수 있습니다.

---

## 3. ScriptableObject 기반 옵저버 패턴 구현

### (1) 이벤트 시스템 정의

```c#
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
    

### (2) 옵저버 스크립트 작성

```c#
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

### (3) 주제 스크립트 작성

```c#
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

## 4. 에디터 설정

1. `GameEvent` ScriptableObject 생성: 프로젝트 창에서 우클릭 > Create > Events > GameEvent.
    
2. 이벤트 트리거와 리스너 설정:
    
    - `EventTrigger` 스크립트를 가진 오브젝트에 `GameEvent`를 연결.
        
    - `EventListener` 스크립트를 가진 오브젝트에 같은 `GameEvent`를 연결.
        

---

## 5. 활용 사례

- **UI 시스템**: 버튼 클릭 시 여러 UI 업데이트 처리.
    
- **게임 이벤트**: 적이 사망하면 점수 증가, 애니메이션 트리거 등.
    
- **전역 알림**: 게임 상태 변경을 전역적으로 알림.
    

---
## 6. ScriptableObject를 활용한 옵저버 패턴의 장점

1. **재사용성**: ScriptableObject는 여러 컴포넌트에서 쉽게 공유할 수 있어 재사용이 용이합니다.
    
2. **유지보수성**: 코드와 데이터가 분리되어 있어, 유지보수가 간단해집니다.
    
3. **유연성**: 이벤트와 리스너의 연결을 에디터에서 설정할 수 있어, 런타임 동작을 유연하게 변경할 수 있습니다.
    
4. **의존성 감소**: ScriptableObject를 사용함으로써 클래스 간 강한 결합을 피할 수 있습니다.
    
5. **메모리 효율**: 필요한 데이터만 메모리에 로드되므로, 메모리 사용량이 적습니다.
    

---

## 7. 결론

ScriptableObject를 활용한 옵저버 패턴은 **간결하고 재사용성**이 높은 코드 구조를 제공합니다. 이를 통해 유니티 프로젝트에서 이벤트 기반 시스템을 효과적으로 설계해 보세요. 필요에 따라 로직을 확장하거나 커스터마이징할 수도 있습니다.