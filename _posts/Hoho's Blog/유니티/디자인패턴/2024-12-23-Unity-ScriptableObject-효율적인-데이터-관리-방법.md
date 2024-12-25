---
title: "Unity ScriptableObject: 효율적인 데이터 관리 방법"
date: 2024-12-23 17:26:08 +09:00
categories:
  - 유니티
  - 디자인패턴
tags:
  - 디자인패턴
  - 유니티
  - ScriptableObject
---

유니티(Unity)에서 데이터 관리 및 공유는 게임 개발의 핵심적인 요소 중 하나입니다. ScriptableObject는 유니티에서 제공하는 강력한 기능으로, 데이터의 효율적인 관리와 재사용성을 극대화할 수 있습니다. 이 글에서는 ScriptableObject의 기본 개념, 장점, 그리고 실사용 예제를 통해 이를 효과적으로 활용하는 방법을 알아보겠습니다.

---

# 1. ScriptableObject란 무엇인가?

ScriptableObject는 유니티에서 제공하는 클래스의 한 종류로, 데이터의 저장 및 관리를 위한 객체입니다. MonoBehaviour와 달리 프로젝트 수준에서 데이터를 저장하고, 메모리 사용을 최소화하면서 데이터를 유지할 수 있습니다.

## 주요 특징

- **씬 독립성**: ScriptableObject는 씬에 종속되지 않으며, 프로젝트의 어디서든 접근 가능.
    
- **재사용성**: 여러 객체에서 동일한 데이터 Asset을 공유 가능.
    
- **메모리 효율성**: 메모리 사용량을 줄이고 퍼포먼스를 향상.
    
- **모듈성**: 데이터와 로직을 분리하여 설계할 수 있음.
	
- **Asset으로 저장**: 게임 모드 외부에서도 변경된 값이 유지됨.

---

# 2. ScriptableObject의 장점

1. **데이터의 중앙화** ScriptableObject를 사용하면 데이터를 중앙에서 관리할 수 있습니다. 예를 들어, 게임의 설정 값이나 캐릭터 속성 데이터를 ScriptableObject에 저장하면, 여러 씬에서 동일한 데이터를 사용할 수 있습니다. 이를 통해 안티패턴으로 구분되기도 하는 싱글톤을 우회하여 설계할 수 있습니다. 
    
2. **Inspector를 통한 직관적인 데이터 편집** ScriptableObject는 유니티의 Inspector 창에서 데이터를 시각적으로 편집할 수 있게 해줍니다. 이는 스크립트를 수정할 필요 없이 데이터를 쉽게 변경할 수 있음을 의미합니다. 협업 시 게임 디자이너가 코드를 만지지 않고도 쉽게 설정 값을 바꿀 수 있는 장점이 있습니다. 
    
3. **코드 간소화** 데이터를 별도의 ScriptableObject로 분리하면, 코드 간결성과 유지보수성이 향상됩니다. 데이터와 로직이 분리되므로, 더 깨끗한 코드 구조를 유지할 수 있습니다.
    
4. **메모리 효율 증가** Monobehaviour는 인스턴스를 각각 모두 생성하지만 ScriptableObject는 프로젝트 수준으로 존재하여 하나를 참조하여 사용하기에 메모리 효율이 증가합니다.
	

---

# 3. ScriptableObject 생성 방법

## 3.1 ScriptableObject 클래스 정의

다음은 ScriptableObject를 정의하는 간단한 코드입니다:

```c#
using UnityEngine;

[CreateAssetMenu(fileName = "NewGameData", menuName = "Game Data")]
public class GameData : ScriptableObject
{
    public string gameName;
    public int maxScore;
}
```

`[CreateAssetMenu]` 어트리뷰트를 사용하면 Unity Editor에서 ScriptableObject를 쉽게 생성할 수 있습니다.
### 유니티6 버전 ScriptableObject 클래스 정의
유니티6부터 ScriptableObject를 더욱 쉽게 생성 할 수가 있습니다.
![](https://i.imgur.com/4hA6B6F.png)

이 방법으로 생성시 자동으로 다음과 같은 코드가 생성됩니다.
```c#
using UnityEngine;

[CreateAssetMenu(fileName = "NewScriptableObjectScript", menuName = "Scriptable Objects/NewScriptableObjectScript")]
public class NewScriptableObjectScript : ScriptableObject
{
    
}
```

---

## 3.2 ScriptableObject Asset 생성

1. Unity Editor에서 `Assets` 폴더로 이동.
    
2. 우클릭 > `Create > Game Data` 선택.
![](https://i.imgur.com/GY1DlFh.png)

3. 생성된 Asset의 이름을 변경하고 Inspector에서 데이터를 편집.
![](https://i.imgur.com/LBmE4R7.png)

---

# 4. ScriptableObject 사용 예제

다음은 ScriptableObject를 사용하는 간단한 예제입니다:

## 게임 데이터 공유

게임의 설정 데이터를 공유하기 위해 ScriptableObject를 활용할 수 있습니다.

### GameManager 코드 예시

```c#
using UnityEngine;

public class GameManager : MonoBehaviour
{
    public GameData gameData;

    void Start()
    {
        Debug.Log($"Game Name: {gameData.gameName}, Max Score: {gameData.maxScore}");
    }
}
```

Inspector에서 `GameData` ScriptableObject를 연결하면, `GameManager`에서 해당 데이터를 참조할 수 있습니다.

---

# 5. 언제 ScriptableObject를 사용해야 할까?

- **전역 데이터 관리**: 여러 씬에서 공통적으로 사용하는 데이터를 저장할 때.
    
- **설정 데이터**: 게임 설정, 난이도, 레벨 정보 등.
    
- **캐릭터/아이템 데이터**: 캐릭터 능력치, 아이템 속성 등.
    

---

# 6. ScriptableObject 사용 시 주의사항

1. **데이터 동기화 문제** ScriptableObject는 참조를 통해 데이터를 공유하므로, 여러 스크립트에서 데이터를 변경하면 의도치 않은 결과를 초래할 수 있습니다.
    
2. **런타임 데이터 관리** ScriptableObject는 주로 고정 데이터를 저장하는 데 적합합니다. 런타임 중 변경해야 하는 데이터는 별도의 복사본을 생성하여 사용해야 합니다. 
    

---

# 7. 결론

ScriptableObject는 유니티에서 데이터 관리와 공유를 간소화하는 데 매우 유용한 도구입니다. 이를 통해 코드 구조를 개선하고, 재사용성을 높이며, 프로젝트의 복잡성을 줄일 수 있습니다. ScriptableObject를 적절히 활용하여 효율적인 게임 개발을 경험해 보세요.