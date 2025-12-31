# TDD (Test Driven Development)
**Aliases**: [[테스트 주도 개발]], [[Red-Green-Refactor]]
**Tags**: #concept/methodology #dev/testing
**Source**: [[20_Learning/10_Topics/Tech_Stack/Antigravity/04_Feature_Planner_Deep_Dive]]

## Definition
테스트 코드를 먼저 작성하고, 이를 통과하는 코드를 구현하며 리팩토링하는 개발 방법론.
**"실패하는 테스트(Red) -> 성공(Green) -> 개선(Refactor)"**의 주기를 따른다.

## Key Principles
1.  **Red**: 동작하지 않는(컴파일도 안 되는) 테스트를 가장 먼저 작성한다.
2.  **Green**: 테스트를 통과하기 위한 '최소한'의 코드를 작성한다. (죄악을 저질러서라도)
3.  **Refactor**: 중복을 제거하고 구조를 개선한다. (기능 변경 없이)

## Usage in Antigravity
`/feature_planner` 워크플로우에서 구현 계획을 세울 때, 이 TDD 사이클을 시뮬레이션하여 안정적인 코드를 작성하도록 유도한다.
