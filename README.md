# 그림으로 보는 Transformer

Jay Alammar의 [The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/)(2018)의 **장 순서를 따라 새로 쓴** 한국어 인터랙티브 해설입니다.
원문의 문장·그림을 옮긴 번역이 아니라, 같은 흐름 위에서 그림 속 숫자를 **브라우저에서 직접 계산해 보도록** 만든 동반 자료입니다.
원문을 한국어로 읽고 싶다면 이미 [nlpinkorean의 번역](https://nlpinkorean.github.io/illustrated-transformer/)이 있습니다.

**🔗 데모: https://dev-jonghoonpark.github.io/illustrated-transformer-ko/**

> [how-ai-works](https://github.com/dev-jonghoonpark/how-ai-works) 시리즈의 일부입니다.
> 다음 단계: [How Transformer Works](https://github.com/dev-jonghoonpark/how-transformer-works) — 파라미터 분포, Post-LN/Pre-LN, 워밍업, KV 캐시

## 다루는 내용

| 절 | 주제 | 인터랙티브 데모 |
|---|---|---|
| §1 | 멀리서 보기 | **상자 열기** — 블랙박스 → 인코더·디코더 → 6층씩. 인코더 **맨 위** 층이 디코더 **모든** 층으로 |
| §2 | 단어가 벡터가 되는 순간 | **임베딩 조회** — 문장 3개 중 하나를 골라 n × 4 행렬 X 만들기(이후 데모 전부의 입력) |
| §3 | 인코더 한 층 안 | **자리 섞기 실험** — 순서를 섞으면 FFN도 **어텐션도 그대로**(순서를 모른다), 토큰을 빼면 어텐션만 달라짐, 위치 인코딩을 켜면 비로소 순서가 구분됨 |
| §4 | 셀프 어텐션의 목적 | **`it`은 어디를 보는가** — "The trophy didn't fit in the suitcase because it was too big/small" (손으로 정한 가중치) |
| §5 | 셀프 어텐션 6단계 | **손계산기** — q·k·v → 내적 → ÷√d<sub>k</sub> → softmax → ×v → 합을 한 단계씩<br>**√d<sub>k</sub> 실험** — 나누지 않으면 d<sub>k</sub>=512에서 softmax가 한 칸에 몰리고 Σp(1−p)가 0으로 |
| §6 | 행렬 한 번에 | `softmax(QKᵀ/√d_k)V`를 전부 계산하고 §5의 결과와 **차이 0** 확인 |
| §7 | 멀티헤드 | 가중치만 다른 **헤드 두 개의 어텐션 맵** → concat → W<sup>O</sup><br>**h 슬라이더** — h=1…32에서 d<sub>k</sub>는 줄고 가중치 합은 **4 × 512² 그대로** |
| §8 | 위치 인코딩 | **PE 히트맵** — 논문식(번갈아) vs 이어 붙이기(sin \| cos) 토글, **내적 곡선**이 기준 위치를 따라 평행이동 |
| §9 | 잔차와 LayerNorm | **Add & Norm 한 칸** — 가지 세기를 바꿔도 출력은 평균 0·분산 1 |
| §10 | 디코더 | **한 단계씩 생성** — "나는 학생 이다" → "i am a student", 인과 마스크와 교차 어텐션 |
| §11 | Linear와 Softmax | **로짓 조절기** — 온도, 모든 로짓에 같은 수를 더해도 확률 불변 |
| §12 | 원-핫 정답과 손실 | **학습 진행 슬라이더** — 분포가 정답 쪽으로 이동하며 교차 엔트로피가 log 6에서 0 근처로 |
| §13 | 탐욕 vs 빔 서치 | **확률 트리** — 탐욕은 "i am"(0.200), 빔 2는 "we are"(0.315)를 찾음 |
| §14 | 더 나아가기 | 논문, Annotated Transformer, 시리즈 다음 자료 |
| §15 | 이해도 퀴즈 | 7문항 |

외부 라이브러리 없이 HTML 파일 하나로 동작합니다.

## 라이선스

원문 「The Illustrated Transformer」가 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)으로 공개되어 있어, 흐름을 빌려 온 이 자료도 같은 조건(CC BY-NC-SA 4.0)으로 공개합니다.
