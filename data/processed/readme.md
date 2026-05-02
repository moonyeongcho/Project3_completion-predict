## Preprocess
### train.csv, test.csv -> train_v8.csv, test_v8.csv
### 전처리 과정
- 파생변수 생성: (count_class, previous_class_count)
- Bool 변환 (re_registration, contest_participation, major_type)
- 칼럼 삭제 및 결측치 처리 (generation, contest_award, idea_contest, major type)
- 다중값 처리(콤마->공백(단 괄호 안의 콤마는 언더바), 띄어쓰기 -> 언더바)
