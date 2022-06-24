From here, we make preprocessing data from crawling data.
```
import pandas as pd  # 데이터 분석을 위한 라이브러리
import numpy as np   # 넘파이 : 숫자 계산
from tqdm import tqdm

# 워닝(버전, 업데이트 문제, 에러는 아님) 코드는 실행되는데 정보. 귀찮을 때 워닝 끄기
import warnings
warnings.filterwarnings('ignore')
book = pd.read_csv('세미 프로젝트 책추천 크롤링ver2.csv') #CSV 불러오기
print(book.shape)  # 행, 열 개수 파악
book.head()
book.rename(columns={'0':'summary'},inplace=True) #불러올때 발생하는 열 제거
book #데이터프레임 확인
book['summary']=book.summary.str.replace('[^ㄱ-ㅎㅏ-ㅣ가-힣 ]','')#한글외 정리 
book['summary']=book.summary.str.replace('^ +','')#공백 시작문자 정리
book['summary']=book.summary.str.replace('책소개','')#책소개 제거
book['summary']=book.summary.replace('',np.nan) #공백 NaN화
book.info()#데이터프레임 정보확인
book = book[['kind','tit','rent','summary']] #작가 제외하고 데이터프레임화
book['rent']=book.rent.str.replace('[^0-9,]','') # rent열의 숫자를 제외한 문자열을 전부 제거
book['rent']=book.rent.str.replace(',',' ') # , 대신 공백문자 
def add(N): # 문자열로 적힌 숫자를 정수형으로 변경하는 함수 작성
    answer = 0    
    for n in N.split( ):
        answer += int(n)
    return answer
book['rent'] = book['rent'].apply(add) # rent에 add 함수 적용
drop_index_list = [] #버릴 행 index를 담을 리스트
for i in range(3000): # 위에서 확인한 데이터프레임 행 개수 
    try:  # try except 로 예외처리
        if len(book.summary[i]) < 30: # summary 열의 항목들 길이가 30이하일 경우
            drop_index_list.append(i) #해당 열의 인덱스 번호를 리스트에 더함
    except:
        continue #예외시 다음 반복으로 이동
drop_index_list # drop할 인덱스 확인
drop_book_df = book.drop(drop_index_list) #인덱스리스트를 기반으로 행드롭
preprocess_df = drop_book_df.reset_index() # 위의 드롭한 데이터프레임을 인덱스를 정렬하여 새 객체로 만듬
```
