---
layout:
  width: default
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
  tags:
    visible: true
  actions:
    visible: true
---

# 데이터 타입 변환

```python
# 거래일시를 datetime 타입으로 변환
df['거래일시'] = pd.to_datetime(df['거래일시'])

# 생년월일을 datetime 타입으로 변환 (③에서 형식 통일 후)
# 남아있는 결측치(NaN)는 자동으로 NaT(Not a Time)로 변환되어 별도 처리가 필요 없습니다.
df['생년월일'] = pd.to_datetime(df['생년월일'])

# 계좌번호는 숫자로 변환하지 않고 문자열(str)로 유지 (앞자리 0 손실 방지, 하이픈 포함)
df['계좌번호'] = df['계좌번호'].astype(str)

df.info()
```

\[출력 결과]

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 9800 entries, 0 to 9799
Data columns (total 10 columns):
 #   Column     Non-Null Count  Dtype         
---  ------     --------------  -----         
 0   record_id  9800 non-null   object        
 1   이름         9800 non-null   object        
 2   주소         9800 non-null   object        
 3   생년월일       9800 non-null   datetime64[ns]
 4   은행명        9800 non-null   object        
 5   계좌번호       9800 non-null   object        
 6   거래일시       9800 non-null   datetime64[ns]
 7   거래유형       9800 non-null   object        
 8   금액         9800 non-null   int64         
 9   거래후잔액      9800 non-null   int64         
dtypes: datetime64[ns](2), int64(2), object(6)
memory usage: 765.8+ KB
```

### **정제 결과 확인**

```python
df.head(5)
```

\[출력 결과]

```
  record_id   이름                  주소       생년월일   은행명           계좌번호  \
0    R00001  최예준  인천광역시 서초구 강남대로 152 1992-11-07  국민은행  130-21-329258   
1    R00002  최민서     성남시 분당구 중앙로 115 1979-12-16  국민은행  877-30-832052   
2    R00003  박예준   광주광역시 서초구 강남대로 98 1963-09-05  기업은행  967-54-733052   
3    R00004  장서준   울산광역시 서초구 문화로 213 1987-06-12  우리은행  821-18-148050   
4    R00005  최예은   부산광역시 분당구 문화로 117 1987-09-24  하나은행  479-55-319684   

                 거래일시 거래유형       금액    거래후잔액  
0 2024-12-27 10:13:36   입금  1154556  2588468  
1 2025-10-18 12:12:24   입금  2355032  4943500  
2 2025-02-14 22:03:09   이체  1166476  3777024  
3 2024-12-20 00:39:48   입금  1927967  5704991  
4 2025-02-18 17:39:16   송금   335671 -5369320  
```

이제 결측치·중복·형식·이상치·타입이 정리된 `df`를 다음 단계(**Step 2. 개인정보 비식별 처리**)에서 사용합니다.
