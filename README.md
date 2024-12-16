# 💸머신러닝 기반 대출 상환 예측 모델
> 본 연구는 개별 고객의  **대출상환 여부**를 예측하는 신용도 평가 모형 개발 프로젝트입니다.

## 프로젝트 수행 과정
> 프로젝트 수행 과정은 다음과 같이 진행됩니다.

1. **데이터 수집**
   - 미국 P2P 대출 기업 **Lending Club의 데이터 셋**
   - 총 141개의 컬럼과 약 300만개의 행으로 구성
   - 컬럼을 개인 정보, 신용 기록, 대출 정보, 정책 자금 파트로 구분
     ![image](https://github.com/Hayeong121/asac_lendingclub/blob/main/image/data_ex.jpg)
     
     
2. **타겟 정의**
   - 프로젝트 목적 구체화하고 적합한 데이터 추출
   - 목적에 맞게 타겟 변수의 값 이진화 작업 수행
     ![image](https://github.com/Hayeong121/asac_lendingclub/blob/main/image/target_definition.jpg)

3. **EDA**
   - 각 개별 변수 기술 통계량 확인, 상관성이 높은 변수와 관련성 파악
   - 인사이트
      - 등급별 대출 금액 및 소득 대비 대출 비율
      - 대출 기간 및 대출 목적
      - 지역별 및 재직 기간별 불량 대출률
     
4. **데이터 전처리**
   - z-score 동일 기준을 정하여 극단값 제거
   - 이상치는 모두 -1로 처리
   - 왜도가 심한 변수들 Box-Cox 변환 후, Min-Max 스케일링 적용
   - 범주형 데이터 원 핫 인코딩, 카운터 인코딩 적용
   - 고유값이 과하게 많은 범주형 데이터는 그룹화하여 새로운 파생 변수 생성
     ![image](https://github.com/Hayeong121/asac_lendingclub/blob/main/image/feature_selection.jpg)

5. **변수 선택**
   - EDA를 통한 변수와 타겟과의 관계 분석 후 변별력 없는 변수 삭제
   - DATA Leakage(시간 순서 위배, 타겟에 정보 제공) 변수들 삭제
   - 가설을 통한 데이터 검증 후 논리적으로 어긋나는 행 삭제
   - 통계기반의 변수선택법을 통해 변별력 없는 변수 삭제
     
6. **모델링/하이퍼파라미터튜닝**
   - 우량 대출 데이터와 불량 대출 데이터의 불균형 해소를 위해 ENN 언더 샘플링 기법 적용
   - 안정된 예측력을 위해 트리기반 알고리즘 모델인 RandomForest, XGBoost, CatBoost, LightGBM을 후보 변수로 하여 평가
   - 튜닝하지 않은 베이스 모델과 튜닝 후 모델 성능 비교
      - 하이퍼파라미터 튜닝방식은 GridSearch/RandomSearch와 Optuna 사용
   - CatBoost 모델이 타 모델에 비해 AUC, F1-score에서 0.01 더 우수한 성능을 보여 최종 모델로 선정
     ![image](https://github.com/Hayeong121/asac_lendingclub/blob/main/image/modeling.jpg)
     
7. **Tableau 대시보드 구축**
   - 최종 모델인 CatBoost로 피처 중요도와 비즈니스 지식을 결합해 중요한 변수 13개에 대한 데이터 추출
   - 두개의 파트로 구성
      - 전체 대출 상태를 모니터링하는 신용 모니터링 시스템
        ![image](https://github.com/Hayeong121/asac_lendingclub/blob/main/image/dashboard1.jpg)
      - 고객의 신용 프로파일을 분석해 대출 가능 여부 파악하는 개인 신용 요인 분석 시스템 (SHAP 값 활용)
        ![image](https://github.com/Hayeong121/asac_lendingclub/blob/main/image/dashboard2.jpg)
   -	데이터 간의 관계를 유기적으로 연결하여 대시보드에서 모든 데이터가 상호작용하며 작동하도록 구성
   
8. **결론**
   -	최종 성능 AUROC 0.74의 모델 개발
   -	개별 고객 맞춤형 분석으로 해석성 제공할 수 있는 대시보드 제작
   -	동일 데이터를 활용한 타 논문 5개 대비 가장 우수한 성능
