---

    NOTE: Copyright (c) 2016 by SAS Institute Inc., Cary, NC, USA.
    NOTE: SAS (r) Proprietary Software 9.4 (TS1M5 MBCS3170)
    NOTE: 이 세션은 X64_10HOME 플랫폼에서 실행되고 있습니다.

    NOTE: 업데이트 된 분석 제품:

      SAS/STAT 14.3
      SAS/ETS 14.3
      SAS/OR 14.3
      SAS/IML 14.3
      SAS/QC 14.3

    NOTE: 추가 호스트 정보:

      X64_10HOME WIN 10.0.16299 Workstation

API로 불러오는 자료의 인코딩에 문제가 있어서 SAS 9.4 (유니코드 지원)으로 실행합니다.

---

#코드
1. Final_extraction_longdata_002.sas
2. Final_extraction_longdata_004.sas
3. Final_extraction_longdata_GDP.sas
4. Final_Preparation.sas
5. Final_Statistics.sas
40. Final_Preparation(modify).sas
50. Final_Statistics(modify).sas
* modify는 논문심사 후 계정명을 추가하여, 투자 및 융자계정을 회귀분석에 추가한 것

#변수
- v21: 예산대비 불용액 비율
- v7: 국회심의과정 중 예산변화율
- v15: 전년도대비 예산증가율
- v19: 예산대비 불용액 비율(전년도)
- v24: 부처예산대비 단위사업 규모
- v100: 인건비 예산비율
- v101: 물건비 예산비율
- v102: 이전지출 예산비율
- v103: 자산취득 예산비율
- v104: 상환지출 예산비율
- v105: 전출금등 예산비율
- v106: 예비비및기타 예산비율
- x16: GDP 성장률
- x9: 정권 연차
- x3: 정권 1년차(더미)
- x31: 정권 2년차(더미)
- x4: 정권 1, 2년차(더미)
- v22: 회계분류
- OFFC_NM: 부처분류

---

longdata_001: 월별 지출집행상황 / 2015-2018 / 예산은 전년도이월액, 추경예산 등을 포함한 현행예산입니다.
longdata_002: 세출/지출 예산편성현황(총액) / 2007-2018
longdata_003: 소관별 세입/세출 결산 현황 / 2007-2018
longdata_004: 세출/지출 결산 현황 / 2007-2016
PreFinal(폐기): longdata_001-longdata_004 full join by 회계연도~단위사업

---

Statistics_001: 기초통계량 및 그래프와 간단한 회귀식
Statistics_002: 논문에 사용될 회귀분석
Statistics_003: 논문에 사용될 기초통계량 및 회귀분석

---

필요한 테이블
  기초통계량
    불용액(연도별, 지출별, 회계별, 소관별)
    세출예산(연도별, 지출별, 회계별, 소관별)
    국회증감비율(연도별, 지출별, 회계별, 소관별)
    월별 집행금액(연도별, 지출별, 회계별, 소관별)
  상관분석
    불용액비율
    전년도 불용액비율
    예산현액 증감비율
    단위사업 세출예산현액/소관당 세출예산현액
    지출분류별 예산 비율
  회귀결과



사용된 변수
    v21: 불용액비율
    v7: 국회증감비율
    X5: 국회증가유무
    x7: 단위사업 10~12월 평균 집행금액 전체와 차이
    x8: 단위사업 1~3월 평균 집행금액 전체와 차이
    x9: 정권년차
    x10: 연말 집행금액 중앙값 초과
    x11: 연말 집행금액 1분위수 초과
    x13: 연초 집행금액 중앙값 초과
    x15: 연초 집행금액 3분위수 미만

    x1: 이명박 정부 기간
    x2: 박근혜 정부 기간
    x3: 정권 1년차
    x4: 정권 초(1,2년차)

  통제변수
    v19: 전년도 불용액비율
    v15: (예산현액-전년도 예산현액)/전년도 예산현액
    v24: 단위사업 세출예산현액/소관 당 세출예산현액
    v100 v101 v102 v103 v104 v105 v106: 지출분류
    v22: 특별회계, 기금, 일반회계, 기타
    OFFC_NM: 소관
    x16: GDP 성장률

---

var001
  v1: 10~12월 평균 집행액
  w1: 전체 단위사업 10~12월 평균 집행액/전체 단위사업 집행액(년)
  v2: 1~3월 평균 집행액
  w2: 전체 단위사업 1~3월 평균 집행액/전체 단위사업 집행액(년)
  v3: 월 평균 집행액
  w3: 단위사업 집행액(년)
  v4: v1/v3
  w4: v1/w3; 10~12월 평균 집행액/단위사업 집행액(년)
  v5: v2/v3
  w5: v2/w3; 1~3월 평균 집행액/단위사업 집행액(년)
  m1: v1 중앙값
  m2: v2 중앙값
  q11: v1 1분위수
  q13: v1 3분위수
  q21: v2 1분위수
  q23: v2 3분위수

var002
  v6: 금년도국회확정안-금년도정부안
  v7: v6/금년도정부안

var003
  v70-v76: 각각 불용액
  v80: 인건비 세출예산현액
  v90: 인건비 지출액
  v81: 물건비 세출예산현액
  v91: 물건비 지출액
  v82: 이전지출 세출예산현액
  v92: 이전지출 지출액
  v83: 자산취득 세출예산현액
  v93: 자산취득 지출액
  v84: 상환지출 세출예산현액
  v94: 상환지출 지출액
  v85: 전츨금등 세출예산현액
  v95: 전출금등 지출액
  v86: 예비비맟가타 세출예산현액
  v96: 예비비및기타 지출액
  v10: 단위사업 세출예산현액
  v11: 단위사업 지출액
  v10*: v8*/v10;항목(별) 예산현액/단위사업 예산현액
  v11*: v9*/v11
  v12*: v7*/v10
  v13*: v7*/v14
  v14*: v7*/v8*
  v14: 단위사업 당 불용액
  v21: v14/v10; 단위사업 당 불용액/단위사업 세출예산현액

var004
  v10: 단위사업 세출예산현액
  w10: 전년도 단위사업 세출예산현액
  v11: 단위사업 지출액
  w11: 전년도 단위사업 지출액
  v14: 단위사업 당 불용액
  w14: 전년도 단위사업 당 불용액
  v15: (v10-w10)/w10; (예산현액-전년도 예산현액)/전년도 예산현액
  v16: (v11-w11)/v11
  v17: v10-w10
  v18: v11-w11
  v19: w14/w10; 전년도 (단위사업 당 불용액/단위사업 세출예산현액)
  v20: v14-w14
  v22: 회계 분류(특별회계, 기금, 일반회계, 기타)

var005
  v23: 소관 당 세출예산현액
  v24: 단위사업 세출예산현액/v23; 단위사업 세출예산현액/소관 당 세출예산현액

var006
  w30: 전년도 (세입)예산현액
  w31: 전년도 (세입)수납액(원)
  w32: 전년도 (세입)증감액(원)
  w33: 전년도 (세출)예산현액(원)
  w34: 전년도 (세출)지출액(원)
  v35: (세출)증감액(원);예산현액-전년도 예산현액
  w35: 전년도 (세출)증감액(원)
  w36: 전년도 다음년도이월액(원)
  w37: 전년도 불용액(원)
  v40: v35/(세출)예산현액(원)
  v41: 불용액(원)/(세출)예산현액(원)
  w41: 전년도 불용액(원)/(세출)예산현액(원)

M02
  x1: 이명박 정부 기간
  x2: 박근혜 정부 기간
  x3: 정권 1년차
  x31: 정권 2년차
  x4: 정권 초(1,2년차)
  x5: 국회증감액 증가
  x6: 국회증감액 감소
  x7: 단위사업 10~12월 평균 집행금액 전체와 차이
  x8: 단위사업 1~3월 평균 집행금액 전체와 차이
  x9: 정권 년차
  x10: 연말 집행금액 중앙값 초과
  x11: 연말 집행금액 1분위수 초과
  x12: 연말 집행금액 3분위수 미만
  x13: 연초 집행금액 중앙값 초과
  x14: 연초 집행금액 1분위수 초과
  x15: 연초 집행금액 3분위수 미만
  x16: GDP 성장률

var007
  CITM_CD_Cate: 지출목 분류
  DUSEAMT_Per: 불용액/세출예산현액
---

### M01
#### reg01
종속변수: v14; 단위사업 당 불용액
독립변수: v6; 금년도국회확정안-금년도정부안
통제변수:
  v10; 단위사업 세출예산현액
  v17: 단위사업 세출예산 차이(전년도)
  v8: 자산항목 세출예산 금액
  v3: 월 평균 집행액
  v1: 10~12월 집행 금액
  v2: 1~3월 집행 금액

#### reg02
종속변수: v21; 단위사업 당 불용액/단위사업 세출예산현액
독립변수: v7; (국회확정안-정부안)/국회확정안
통제변수:
  v19; 전년도 (단위사업 당 불용액/단위사업 세출예산현액)
  v15; (예산현액-전년도 예산현액)/전년도 예산현액
  v24; 단위사업 세출예산현액/소관 당 세출예산현액
  v10*: 지춟분류 세출예산현액/전체 세출예산현액
  w4; 10~12월 평균 집행액/단위사업 집행액(년)
  w5; 1~3월 평균 집행액/단위사업 집행액(년)
  v22; 특별회계, 기금, 일반회계, 기타 더미변수
  Year fixed
  소관 fixed

#### reg03-01/단위사업 기준 전년도 불용액이 올해 예산 감소에 영향을 미치는가?
종속변수: v15; (예산현액-전년도 예산현액)/전년도 예산현액
독립변수: v19; 전년도 (단위사업 당 불용액/단위사업 세출예산현액)
통제변수:
  v24; 단위사업 세출예산현액/소관 당 세출예산현액
  v10*: 지춟분류 세출예산현액/전체 세출예산현액
  v22; 특별회계, 기금, 일반회계, 기타 더미변수
  Year fixed
  소관 fixed

#### reg04/국회증액이 많은 예산이 불용액이 적다.
  국회증액: 쪽지, 비효율; 불용액 증가
  국회증액: 수요가 존재, 준비된 사업; 불용액 감소
종속변수: v21; 단위사업 당 불용액/단위사업 세출예산현액
독립변수: v7; (국회확정안-정부안)/국회확정안
  더미변수: x6(if v7<0 1 else then 0)
  교차분석: 더미변수
통제변수:
  v19; 전년도 (단위사업 당 불용액/단위사업 세출예산현액)
  v15; (예산현액-전년도 예산현액)/전년도 예산현액
  v24; 단위사업 세출예산현액/소관 당 세출예산현액
  v10*: 지춟분류 세출예산현액/전체 세출예산현액
    10~12월 평균 집행액/단위사업 집행액(년)-전체{10~12월 평균 집행액/단위사업 집행액(년)}
    1~3월 평균 집행액/단위사업 집행액(년)-전체{1~3월 평균 집행액/단위사업 집행액(년)}
  v22; 특별회계, 기금, 일반회계, 기타 더미변수
  Year fixed
  소관 fixed

#### reg05/
  국회증액: 쪽지, 비효율; 불용액 증가
  국회증액: 수요가 존재, 준비된 사업; 불용액 감소
종속변수: v21; 단위사업 당 불용액/단위사업 세출예산현액
독립변수: v7; (국회확정안-정부안)/국회확정안
  교차분석: 초기/후기 평균 집행액
통제변수:
  v19; 전년도 (단위사업 당 불용액/단위사업 세출예산현액)
  v15; (예산현액-전년도 예산현액)/전년도 예산현액
  v24; 단위사업 세출예산현액/소관 당 세출예산현액
  v10*: 지춟분류 세출예산현액/전체 세출예산현액
    10~12월 평균 집행액/단위사업 집행액(년)-전체{10~12월 평균 집행액/단위사업 집행액(년)}
    1~3월 평균 집행액/단위사업 집행액(년)-전체{1~3월 평균 집행액/단위사업 집행액(년)}
  v22; 특별회계, 기금, 일반회계, 기타 더미변수
  Year fixed
  소관 fixed

#### reg06/정권 초기에 불용액이 많다.
  국회증액: 쪽지, 비효율; 불용액 증가
  국회증액: 수요가 존재, 준비된 사업; 불용액 감소
종속변수: v21; 단위사업 당 불용액/단위사업 세출예산현액
독립변수: v7; (국회확정안-정부안)/국회확정안
  더미변수: x4(if 2008, 2009, 2013, 2014 then 1 else 0)
  교차분석: 더미변수
통제변수:
v19; 전년도 (단위사업 당 불용액/단위사업 세출예산현액)
v15; (예산현액-전년도 예산현액)/전년도 예산현액
v24; 단위사업 세출예산현액/소관 당 세출예산현액
v10*: 지출분류 세출예산현액/전체 세출예산현액
  10~12월 평균 집행액/단위사업 집행액(년)-전체{10~12월 평균 집행액/단위사업 집행액(년)}
  1~3월 평균 집행액/단위사업 집행액(년)-전체{1~3월 평균 집행액/단위사업 집행액(년)}
v22; 특별회계, 기금, 일반회계, 기타 더미변수
Year fixed
소관 fixed


---

1. 불용액이 정권초기에 더 많은가?

종속변수: v21; 불용액비율
독립변수: x9; 정권년차(Dummy)
통제변수: v19 v15 v24 v100 v101 v102 v103 v104 v105 v106 v22(Dummy) OFFC_NM(Dummy) FSCL_YY(Dummy) or x16

종속변수: v21; 불용액비율
독립변수: x9; 정권년차
통제변수: v19 v15 v24 v100 v101 v102 v103 v104 v105 v106 v22(Dummy) OFFC_NM(Dummy) FSCL_YY(Dummy) or x16

2. 국회에서 증액된 예산(쪽지예산)은 비효율적으로 집행되는가?

종속변수: v21; 불용액비율
독립변수: v7; 국회증감비율 x5; 국회증가(Dummy) / 0 포함
통제변수: v19 v15 v24 v100 v101 v102 v103 v104 v105 v106 v22(Dummy) OFFC_NM(Dummy) FSCL_YY(Dummy) or x16

종속변수: v21; 불용액비율
독립변수: v7; 국회증감비율 x5; 국회증가(Dummy) / 0 제외
통제변수: v19 v15 v24 v100 v101 v102 v103 v104 v105 v106 v22(Dummy) OFFC_NM(Dummy) FSCL_YY(Dummy) or x16

3. 연말에 집중해서 집행되는 예산은 비효율적인가?

종속변수: v21; 불용액비율
독립변수: x7
통제변수: v19 v15 v24 v100 v101 v102 v103 v104 v105 v106 v22(Dummy) OFFC_NM(Dummy) FSCL_YY(Dummy) or x16

종속변수: v21; 불용액비율
독립변수: v7 x5(Dummy) x11(Dummy) / 1분위수 초과
통제변수: v19 v15 v24 v100 v101 v102 v103 v104 v105 v106 v22(Dummy) OFFC_NM(Dummy) FSCL_YY(Dummy) or x16

종속변수: v21; 불용액비율
독립변수: v7 x5(Dummy) x10(Dummy) / 중앙값 초과
통제변수: v19 v15 v24 v100 v101 v102 v103 v104 v105 v106 v22(Dummy) OFFC_NM(Dummy) FSCL_YY(Dummy) or x16

4. 연초에 집행되지 않는 예산은 비효율적인가?

종속변수: v21; 불용액비율
독립변수: x8
통제변수: v19 v15 v24 v100 v101 v102 v103 v104 v105 v106 v22(Dummy) OFFC_NM(Dummy) FSCL_YY(Dummy) or x16

종속변수: v21; 불용액비율
독립변수: v7 x5(Dummy) x15(Dummy) / 3분위수 미만
통제변수: v19 v15 v24 v100 v101 v102 v103 v104 v105 v106 v22(Dummy) OFFC_NM(Dummy) FSCL_YY(Dummy) or x16

종속변수: v21; 불용액비율
독립변수: v7 x5(Dummy) x13(Dummy) / 중앙값 초과
통제변수: v19 v15 v24 v100 v101 v102 v103 v104 v105 v106 v22(Dummy) OFFC_NM(Dummy) FSCL_YY(Dummy) or x16
