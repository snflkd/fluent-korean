# fluent-korean (Claude Code output-style)

이 플러그인은 LLM, 특히 클로드와 코딩 에이전트가 명확한 한국어를 유창하게 구사하도록 지시하는 출력 스타일입니다.
수려한 문체보다는 확실한 의미 전달을 추구하며, 비교적 자연스러운 문장을 목표로 합니다. 

한국어로 명령을 입력하거나, 작업 결과물에 한국어가 포함된 경우에 유용합니다.
클로드 코드 환경에 적합한 Plugin이지만, 글쓰기 지침에 해당하는 텍스트를 적절하게 활용하면 다른 AI에도 적용할 수 있습니다.

> **EN**: A Claude Code output style that stops Claude from writing broken machine-Korean: dropped particles and endings, telegraphic noun strings, and metaphor-swapped vocabulary. It prioritizes clear, unambiguous meaning over elegant prose, aiming for reasonably natural sentences. Ships as two variants: one that keeps Claude Code's coding instructions, and one without them. The guideline text itself can also be pasted into other AI environments as a plain writing instruction. Written by a Korean-literature major; the Korean parts of this README are human-written. If you're at Anthropic and want Claude to write proper Korean, the maintainer would love to hear from you.

## 왜 필요한가

최근 AI 모델은 한국어를 능숙하게 쓰지 못하는데, 토큰 축약을 위해 말을 간결하게 하도록 구현된 코딩 에이전트들은 더 그렇습니다. 

게다가 클로드는 코드가 아닌 채팅에서도, 오푸스, 페이블처럼 버전과 성능이 올라갈수록 한국어 구사 능력은 오히려 하락하는 것이 체감됩니다.

따라서 LLM의 답변을 사용자가 이해하기도 어렵고, 한국어 결과물의 완성도도 낮아집니다.

다중 에이전트나 하네스를 활용하는 상황에서 이들이 한국어로 프롬프트나 자료를 주고받는다면 작업의 퀄리티 문제가 더 심각해집니다.

더불어 사고(추론) 기능을 활성화한 경우 한국어 품질이 낮아지기 쉽고, 사고 과정 자체가 낮은 한국어 품질에 영향을 받기도 쉽습니다. (자세한 설명을 원하신다면 하단에서 '원리' 문서 링크를 찾아주세요)

이런 현상을… 최대한 개선하는 도구가 바로 fluent-korean입니다.

*(비포 애프터 사진 추가 예정)*


## 이 도구의 역할과 범위, 다른 도구와의 차이점

1. 이 도구는 기본적으로 output-style이므로, 시스템 프롬프트에 추가되어 한국어를 올바르게 사용하도록 **사전에** 규율합니다. (자세한 설명을 원하신다면 하단에서 '원리' 문서 링크를 찾아주세요)
2. 이 도구는 의미가 손실되지 않는 명확한 한국어 표현을 목표로 합니다. 따라서 번역체 교정, AI 표현 최소화, 맞춤법 오류 제거와 같은 skill을 원하신다면 [여기](https://github.com/epoko77-ai/im-not-ai), [여기](https://github.com/DaleSeo/korean-skills), [여기](https://github.com/NomaDamas/k-skill/blob/main/docs/features/korean-humanizer.md)를 참조해주세요.


## 설치 방법과 구성 (클로드 코드 CLI 환경)

설치 방법은 다음과 같습니다.

```
/plugin marketplace add snflkd/fluent-korean
/plugin install fluent-korean@fluent-korean
```
1. 클로드 코드를 실행한 후, 위 명령어 두 줄을 입력하세요. 
2. 설치한 뒤에 /config 메뉴에서 output-style을 찾아 둘 중 하나를 선택하세요. (기본적으로 출력 스타일 두 가지가 들어 있습니다.) 
3. output-style 특성상, config에서 변경 사항을 적용한 후에 새 세션을 시작하거나 `/clear`를 사용해야 합니다.

다음 두 가지 출력 스타일이 포함되어 있습니다.

`fluent-korean` : 코딩 지침을 유지합니다. 코딩 작업에 사용하세요.

`fluent-korean-not-coding` : 코딩 지침이 제거되어 있습니다. 이 에이전트가 직접 코드를 변경하지 않을 때 사용하세요.


## Claude Code CLI 외 다른 환경에서 사용하는 방법

기본적인 원리는 간단합니다. `plugins/fluent-korean/output-styles/`의 md 파일 중 원하는 것을 선택해서, 파일 본문을 적절한 자리에 삽입하면 됩니다.

(따라서 Claude Code CLI 환경이라도, 플러그인을 설치하지 않고 md 파일을 목표 디렉토리 `~/.claude/output-styles/`에 붙여넣어도 됩니다. 다음 단락에 설명되어 있는 세부 동작 설정을 원하신다면 이렇게 해주세요.)

다양한 환경에서 적절하게 사용하는 방법은 추후에 추가하겠습니다만, 구체적인 동작은 환경에 따라 차이가 나기 때문에, 설치와 활용 방법은 가능하면 LLM과 상의해주세요. 


- 클로드 웹, 클로드 데스크탑 앱에서 채팅, 코워크를 쓴다면? -> 좌측 하단 설정에서 개인별 지침 추가 가능(채팅은 프로필이나 일반 탭, 코워크는 협업 탭) 여기에 md 파일 본문 텍스트를 추가하면 됨. 개별 프로젝트에만 적용하고 싶으면 프로젝트 지침에 두면 됨.
- 클로드 데스크탑 앱에서 클로드 코드를 쓴다면? -> 여기선 config를 입력하면 나오는 메뉴에 output-style 바꾸는 항목이 없어서, 'CLAUDE.md나 settings.json 혹은 settings.local.json ' 중 상황에 맞는 것을 골라서 잘 적용하셔야 함.
- CLI인데, 매번 안 바꿔도 알아서 사용되길 원한다면? -> 상황에 따라  'settings.json 혹은 settings.local.json'  같은 설정을 찾아서 기본으로 적용되는 output-style을 바꾸면 됨
- 시스템프롬프트 말고 적절한 선에서 사용되길 원한다면? or 클로드 환경이 아니라면? -> md 파일이나 본문 텍스트를 적절하게 제공하고, 알아서 잘 참조하라고 하거나, 스킬로 만들어서 잘 쓰시거나 하면 됨.


## 세부 동작 - 저는 Claude가 이랬으면 좋겠어요.

세부 동작 중에서 원하는 것을 골라서, 블록 내의 텍스트를 지침 끝에 추가하면 됩니다.

따라서 CLAUDE.md, 개인별 Claude 지침, 프로젝트 지침 등을 사용하신다면, 해당 지침의 텍스트 말미에 블록 내 텍스트를 붙여넣으면 됩니다.

(기본 plugin 설치 등 방법으로) md 파일을 직접 디렉토리에 배치하셨다면, md 파일 본문 말미에 블록 내 텍스트를 붙여넣으면 됩니다.

다만 Claude Code CLI 환경에서 plugin으로 설치하신다면 업데이트시 파일이 덮어쓰기 될 수 있으니 유의해주세요.

- **예의바르게 만들어주고 싶다면?** ('사용자님' 부분을 '원하는 호칭'으로 바꿔주세요) →

  ```
  - 사용자를 '사용자님'이라고 호칭하고, 선어말어미와 어말어미, 조사, 공손어휘를 통해 사용자께 높임말을 사용합니다. 사용자에게 반말이나 비존대로 발화하지 않습니다. [네가 한 말대로 내가 진행할까? → '사용자님'께서 하신 말씀대로 제가 진행하면 될까요?]
  ```

- **오푸스나 페이블이 비유적인 어휘, 사전 속에나 있는 단어를 너무 많이 쓴다면?**->

  ```
  - 한국어 사전에 있는 어휘이며 뜻이 명확하더라도, 사용 빈도가 낮아서 통용되지 않는 어휘를 사용하면 소통의 효율성이 오히려 낮아지니 자제합니다. 대신 의미가 명확하며 실제로 통용되는 어휘를 우선적으로 선택합니다.
  ```

- **내가 초보 개발자라면?** →

  ```
  - 코딩에 관해서는 초보 개발자가 이해할 수 있도록 서술하고, 현장감이 과한 구어체 표현은 더욱 자제합니다. (박아넣다, 치우다, 얹다 등)
  ```

- **나에 대한 보고 말고, 다른 한국어 출력에도 적용되어야 한다면?** →

  ```
  - 한국어로 출력되는 모든 결과물에도 이 지침들을 적용합니다.
  ```

- **애들이 자꾸 영어를 뱉는 게 짜증난다면?** ->

  ```
  - 항상, 한국어로 사고하고, 한국어로 보고하고, 한국어로 출력합니다.
  ```

- **교정이 잘 안 된다면?** ->

  ```
  - 답변을 사용자에게 출력하기 직전에, 위의 지침들에 어긋난 부분을 반드시 점검하고 수정한 후에 출력합니다.
  ```



## 유의점

- **토큰을 조금 더 씁니다.** 생략된 여러 문장 성분과 형태소를 복원하므로 메시지 토큰 사용이 조금 늘어나고, 컨텍스트 차지량이 늘어납니다. (또한 시스템 프롬프트에 지침이 더해지므로 매 세션마다 아주 조금 토큰을 더 사용합니다. )
- 직접 설치시, 파일 이름과 설정 대소문자에 유의해주세요. 이것 때문에 설정이 안 되는 케이스가 보고되어 있습니다.
- 서브에이전트 동작은 상황에 맞추어 확인하셔야 합니다. 기본적으로 서브에이전트에게 입력되는 프롬프트도 한국어라면 이 output-style을 준수하도록 되어 있고, '영어로 작성할 것을 한국어로 작성하지 않는다'라는 취지의 조항이 있으나, 이 조항들이 실제로 얼마나 지켜질지는 상황에 따라 많이 다르니, 필요하다면 직접 관찰해서 올바로 동작하도록 수정해주세요.

## 원리

모델이 한국어를 왜 못 쓰는지, 얼마나 어떻게 못 쓰는지, 왜 잘 쓰게 하기 어려운지, 어떻게 교정하는지는 다음 문서에서 다룹니다. *[원리 문서](https://docs.google.com/document/d/1FGthxxrCVtjCfPG6qZSI-F0_DBOz6iNNlbVBUJ3xw2M/edit?usp=sharing) (작성중)*

어휘 Priming을 조절할 수 있도록, output-style을 지시하는 프롬프트 본문도 해당 지침을 최대한 준수하여 작성했습니다.


## 기타

국어국문학 전공자가 만들었으며, 이 README의 한국어 부분은 전부 인간이 손으로 쓰고 AI에게 자문을 구해 수정했습니다.

더 나은 설치 방법을 아시는 분이 있다면 적극적으로 의견을 개진해주십시오.

그리고 앤트로픽 보아라. 이 Repo를 보면 꼭 연락 주십시오. Claude에게 한국어가 무엇인지 알려주겠습니다.


## 라이선스

MIT
