This part is visualize program by saved data.
```
import sys
from PyQt5.QtWidgets import *
from PyQt5 import uic, QtCore, QtWidgets
from PyQt5.QtCore import QCoreApplication
from numpy import dot
from numpy.linalg import norm
import pandas as pd
from sklearn.metrics.pairwise import cosine_similarity
from sentence_transformers import SentenceTransformer
from sklearn.feature_extraction.text import CountVectorizer
from tqdm import tqdm
import urllib.request
from sentence_transformers import SentenceTransformer

model = SentenceTransformer('sentence-transformers/xlm-r-100langs-bert-base-nli-stsb-mean-tokens')
major_dict = {#인문/사회과학
'국어국문학과' : '국어 국문학 문학 현대시 작품 한국 모색 고대 국어학 문자 문화 비평 역사 소설 문학사 출판물 언어 고전 정서법 모색 한국어 인문학 이야기 창작'
,'영어영문학부' : '영어 영미 독해 미국 인문학 문학 감상 번역 담화 서양 영문학 영국 소설 텍스트 고전 회화 어학 문장 문학사 신문 영시 교재 베스트셀러 장르'
,'일어일문학부' :  '일본어 일본 독해 번역 일문학 일본사 텍스트 회화 문학 인문학 동아시아 일본문화 역사 근현대사 일식'
,'사학과' : '역사 사료 역사학 답사 사학 모임 한국사 동양사 지역학 시대사 문화유산 역사관 동서양 전통 문화 서양사 탐구 고고학 분류 연구자 문화'
,'경제학과' : '경제학 금융 시장 상품 투자 거래 거시경제 시장 미시경제 재정정책 화폐 경제사 가격 무역 외환시장 재무 신용 독점 제도 통계 임금'
,'법학과' : '헌법 인문 자치 회사 보호 재판 형벌 행정 법원 로스쿨 변호사 판사 판결 검사 소송 인권 법인 논리 법학 재산 자유 권리 판례 불법 민법 형법 처벌'
,'행정복지학부' : '행정 정책 정책학 공무원 인문 민법 행정학 공직 조직론 정부 복지 사고 공익 합리 법학 소통 구조사 민원 기관 공공근로'
,'국제지역학부' : '정치 국제 경제 동아시아 세계 미국 중국 국제법 다국적 다문화 인권 시장 개발도상국 테러리즘 외교 소개 정부 전쟁 무역 안보 '
,'중국학과' : '중국 동아시아 중국문화 중국어 정책 무역 중국사 역사 대중정책 동양 사회주의 중국어 한자 동양 회화 중화사상 공산당'
,'정치외교학과' : '정책 정치 정부 국제 외교 관계 국제법 세계 동양 서양 협상 유엔 해외 무역 조약 체결 미래 무역 외환 전쟁 안보'
,'유아교육과' : '교육 초응 아동 기초 상담 교육과정 윤리 교육심리학 교육행정 성취 자녀 교육사 특수교육 보육 인적자원 부모 청소년 학업 교직 학교폭력'
,'패션디자인학과' : '기획 디자인 프로젝트 미디어 이미지 디자이너 매체 전시회 공간 스튜디오 제품 드로잉 작가 입체 실기 조형 제작 아이덴티티 발상'

#경영대학
,'경영학부' : '비즈니스 경영 사업 창업 세법 재무제표 투자 주식 회계 자산 법인 의사결정 자본 성장 원가 증권 리더십 매매 세무 금융'
,'국제통상학부' : '비즈니스 무역 투자 국제관계 협상 미국 중국 국제법 개발도상국 시장 자본 주식 법인 자산 금융 사업 창업'

#자연과학대학
,'응용수학과' : '수학 증명 연구 공식 함수 미적분학 숫자 수식 산업 응용수학 수치 통계 행렬 방정식 기하학 선형대수 현대수학 금융수학 수치 확률'
,'물리학과' : '물리학 역학 에너지 연구 실습 고체물리 재료 양자역학 수학 법칙 이론 자연과학 광학 통계역학 상대성이론 수리물리 전자기학 실험 대학원 학자 열역학 '
,'미생물학과' : '미생물 세균 생명 생물 실험 감염 유전체 바이러스 유전 면역 배양 항균 항원 박테리아 질환 증식 항체 멸균'
,'간호학과' : '보건 의학 간호사 의료 실습 생명 윤리 병리학 해부학 약리학 병원 건강 의학용어 보건교육 예방 치료 병리 약리 공중보건 질병'
,'과학시스템시뮬레이션학과' : '시스템 실험 자연과학 연구 컴퓨터 시뮬레이션 모델 이론 컴퓨팅 클러스터 4차산업'
,'화학과' : '일반화학 화학 일반물리학 실험 프로그램 공정 물리화학 촉진 유기화학 고분자화학 촉매 정밀반응 고분자 에너지공학'

#공과대학
,'전기공학부' : '전기 전자 반도체 회로 증폭 저항 출력 제어 소자 전력 로그 공학 동작 확률 신호 집적회로 구현 공업 공학'
,'기계공학부' : '기계공학 기계 에너지 유체 재료 역학 열역학 방정식 동역학 자동차 회로 동력 운동량 모멘트 선형대수 벡터 공업 공학'
,'에너지수송시스템공학부' : '공학 공업 수송 에너지 시스템 물류 기계 자동차 바이오 냉동 설계 운송 연구 공조 콜드체인'
,'화학공학과' : '에너지 물질 소재 효소 공학 유동 화학공학 연구원 열역학 고분자 반응 분자 분리 수지 변환 바이오 나노 재료 반도체 공정'
,'공업화학고분자공학부' : '공업 화학 공학 분자 공정 물리화학 일반화학 설계 촉진 유기화학 에너지 정밀'
,'나노융합공학과' : '나노 융합 공업 실험 재료 반도체 전자 잉크 전지 물리 화학 고분자 소재 공학 광학 '
,'시스템경영안정공학부' : '시스템 기술경영 설계 운영 분석 평가 통합 수리 과학 공학 제조 최적화 데이터 전략'
,'소방공학과' : '화학 공학 소방 전기 안전 설계 열역학 유체역학 시뮬레이션 설비 약제 실험 예방 실험 미적분학'
,'융합소재공학부' : '융합 소재 재료 실험 산업 기계 광학 물성 실험 공업 공학 고분자 신소재 광학 화학 물리'
,'건축공학과' : '도시 건축물 공간 건축 조형 디자인 시공 설비 도면 설계 제작 프로젝트 시설 건축가 건설 건물'
,'지속가능공학부' : '토목 공해 설계 시설 지형 공간 공업 공학 설비 전기 생태 도로 항만 발전소 플랜트'

#수산과학대학
,'식품과학부' : '식품 과학 영양소 영양학 식품공학 화학 공학 설계 미생물 공정 성분 냉장 바이오 발효 생산 저온'
,'생물공학과' : '공업 공학 생명공학 공정 생명 나노기술 물질 효소 분리 반응 에너지 생물 분자 바이오 유동 촉매 미생물 수지 유체역학 합성 석유 제어기 용액'
,'해양시스템관리학부' : '수산 해양 바다 시스템 관리 연구 기후변화 친환경 항만 어업 수산물 어업 청정 환경'
,'수산생명과학부' : '수산 생명 자연과학 연구 실험 생물 반응 미생물 해양 바다 미생물 바이오'
,'수해양산업교육과' : '교육 윤리 해양 수산 교육사 교직 학업 바다 교육과정 해양 항해 냉동 고등학교 가공'
,'수산생명의학과' : '의학 생명 수산 바다 생물 실험 관리 해부학 생리학 조직학 면역학 공중보건 식품위생 질병 해양'
,'해양수산경영경제학부' : '해양 수산 운송 무역 경제 자원 산업 국제 유통 물류 바다 선박 융합 항만'

#환경/해양대학
,'해양공학과' : '공학 물류 도시 해양 설비 개발 항만 운송 선박 해안 구조물 환경 재해 방재'
,'지구환경시스템과학부' : '환경 연구 물리 자연과학 청정 지구과학 대기 해양 지질 보호 자연 재해 오염 생태 기후'
,'에너지자원공학과' : '석유 풍력 에너지 천연가스 산업 공학 공업 지열 수소 신재생 광물 산업원료 귀금속 희소전략광물'

#정보융합대학
,'데이터정보과학부' : '정보 데이터 빅데이터 통계 학습 평가 모델 설계 확률 알고리즘 수학 마이닝 전처리 회귀분석 딥러닝 자연어 시스템'
,'미디어커뮤니케이션학부' : '뉴스 미디어 저널리즘 공중 윤리 광고 마케팅 언론 콘텐츠 이슈 대중문화 영상 메시지 소통 매체'
,'스마트헬스케어학부' : '의료기기 스포츠 재생 의공전산 치료기기 병원 설비 연구 공학'
,'전자정보통신공학부' : '정보 공학 공업 통신 전자 디지털 전력 네트워크 링크 로봇 증폭 회로 동작 나노 반도체 접합 전자공학 확률 저항 제어'
,'조형학부' : '디자인 설계 제품 설비 인테리어 디자이너 공학 시각 광고 편집 일러스트 타이포그라피'
,'컴퓨터공학부' : '소프트웨어 하드웨어 수학 행렬 컴퓨터 공학 산업 알고리즘 자료구조 프로그래밍 구조 인공지능 딥러닝 머신러닝 학습'

#미래융합대학
,'평생교육상담학과' : '교육 심리학 상담 발달심리 청소년 정신병 성격 이상심리 범죄심리 진로 직업'
,'기계조선융합공학과' : '기계 조선 공학 산업 항만 선박 운송 설계 설비 기자재 수송 냉동 해운'
,'전기전자소프트웨어공학과' : '전기 전자 소프트웨어 공학 실험 설계 회로 산업 신호 저항 정보 프로그래밍'
,'공공안전경찰학과' : '경찰 범죄 수사 교정 법학 민간 경비 경찰관 조직 행정 제도 검거 조직 예방 심리 치안 보호 법률 경찰학 체력 헌법 대책 법규 방지'
}
major_list = list(major_dict.keys())
embedding_major = {}
for i in major_list:
    major_emb = model.encode(major_dict.get(i))
    embedding_major.setdefault(i, major_emb)

def cos_sim(A, B): #내적으로 코사인 유사도 구함
    return dot(A, B)/(norm(A)*norm(B))
def find_answer(book, department): #책 데이터를 지정할 수 있는 함수
    book_list = [] #책 데이터 리스트
    embedding = embedding_major.get(department) 
    book['score'] = book.apply(lambda x: cos_sim(x['embedding'], embedding), axis=1) #입력받은 임베딩값과 기존 임베딩 값 코사인 유사도로 점수 생성
    sort_index_list = book['score'].sort_values(ascending= False).index # 점수값을 기준으로 내림차순 정렬 후 인덱스 반환
    for i in range(10):
        book_list.append(book.loc[sort_index_list[i]]['tit']) #반환한 인덱스에 해당하는 책 제목값 상위 10개 출력
    return book_list 
def book_recommend(): #책 추천 시스템
    major = input('학과를 입력하세요:') #학과, 학과는 딕셔너리 자료형의 키값으로 사용됨
    grade = input('학년을 입력하세요:') #학년
    if int(grade) <= 2: # 2학년 이하
        grade_1_2_book_data = preprocess_df[preprocess_df['kind'].str.contains('문학') | preprocess_df['kind'].str.contains('인문／사회') | preprocess_df['kind'].str.contains('경제／경영') | preprocess_df['kind'].str.contains('가정과생활') | preprocess_df['kind'].str.contains('국어와외국어') | preprocess_df['kind'].str.contains('장르문학') | preprocess_df['kind'].str.contains('대학교재') | preprocess_df['kind'].str.contains('자연과과학') | preprocess_df['kind'].str.contains('컴퓨터와인터넷') | preprocess_df['kind'].str.contains('예술／대중문화')] #데이터프레임 분할
        grade_1_2_rec = find_answer(grade_1_2_book_data, major) #데이터프레임을 분할 후 데이터 지정함수에 넣어줌
        return str(grade_1_2_rec)
    elif int(grade) >= 3: #3학년 이상
        grade_3_4_book_data = preprocess_df[preprocess_df['kind'].str.contains('자기관리') | preprocess_df['kind'].str.contains('인문／사회') | preprocess_df['kind'].str.contains('경제／경영') | preprocess_df['kind'].str.contains('가정과생활') | preprocess_df['kind'].str.contains('국어와외국어') | preprocess_df['kind'].str.contains('대학교재') | preprocess_df['kind'].str.contains('자연과과학') | preprocess_df['kind'].str.contains('컴퓨터와인터넷') | preprocess_df['kind'].str.contains('예술／대중문화') | preprocess_df['kind'].str.contains('해외원서')]
        grade_3_4_rec = find_answer(grade_3_4_book_data, major)
        return str(grade_3_4_rec)

data = pd.read_csv('data_for_app') 
#change file name's for data

data.drop(['Unnamed: 0', 'Unnamed: 0.1', 'level_0', 'index'], axis = 1 ,inplace = True)

data['embedding'] = data.embedding.str.replace('[','')
data['embedding'] = data.embedding.str.replace(']','')
data['embedding'] = data.embedding.str.replace('\n',' ')

for i in range(len(data['embedding'])):
    data['embedding'][i] = data['embedding'][i].split()

for i in range(len(data['embedding'])):
    for j in range(768):
        data['embedding'][i][j] = float(data['embedding'][i][j])

preprocess_df = data
grade_1_2_book_data = preprocess_df[
                preprocess_df['kind'].str.contains('문학') | preprocess_df['kind'].str.contains('인문／사회') | preprocess_df[
                    'kind'].str.contains('경제／경영') | preprocess_df['kind'].str.contains('가정과생활') | preprocess_df[
                    'kind'].str.contains('국어와외국어') | preprocess_df['kind'].str.contains('장르문학') | preprocess_df[
                    'kind'].str.contains('대학교재') | preprocess_df['kind'].str.contains('자연과과학') | preprocess_df[
                    'kind'].str.contains('컴퓨터와인터넷') | preprocess_df['kind'].str.contains('예술／대중문화')]
grade_3_4_book_data = preprocess_df[
                preprocess_df['kind'].str.contains('자기관리') | preprocess_df['kind'].str.contains('인문／사회') |
                preprocess_df['kind'].str.contains('경제／경영') | preprocess_df['kind'].str.contains('가정과생활') |
                preprocess_df['kind'].str.contains('국어와외국어') | preprocess_df['kind'].str.contains('대학교재') |
                preprocess_df['kind'].str.contains('자연과과학') | preprocess_df['kind'].str.contains('컴퓨터와인터넷') |
                preprocess_df['kind'].str.contains('예술／대중문화') | preprocess_df['kind'].str.contains('해외원서')]

form_class = uic.loadUiType("Semi2.ui")[0]

#메인 윈도우 클래스
class WindowClass(QMainWindow, QWidget, form_class) :
    #초기화 메서드
    def __init__(self) :
        super().__init__()
        self.setupUi(self)
        self.setWindowFlag(QtCore.Qt.FramelessWindowHint)
        #pushButton (시작버튼)을 클릭하면 아래 fuctionStart 메서드와 연결 됨.
        self.initUI()
        self.show()
        
    def initUI(self):
        self.pushButton.clicked.connect(self.functionStart)
        self.Closebutton.clicked.connect(self.close)
        
    # 시작버튼을 눌렀을 때 실행되는 메서드
    def functionStart(self):
        major = self.lineEdit.text()
        grade = int(self.lineEdit_2.text())
        if major not in embedding_major:
            self.textEdit.setText('잘못된 학과 입력입니다.')
        if grade <= 2:  # 2학년 이하
            grade_1_2_rec = find_answer(grade_1_2_book_data, major)# 데이터프레임을 분할 후 데이터 지정함수에 넣어줌
            self.textEdit.setText(str(grade_1_2_rec))     
        elif grade >= 3:  # 3학년 이상
            grade_3_4_rec = find_answer(grade_3_4_book_data, major)
            self.textEdit.setText(str(grade_3_4_rec))
 
#코드 실행시 GUI 창을 띄우는 부분
#__name__ == "__main__" : 모듈로 활용되는게 아니라 해당 .py파일에서 직접 실행되는 경우에만 코드 실행
if __name__ == "__main__" :
    #app = QApplication(sys.argv) # argument
    app = QtCore.QCoreApplication.instance()
    if app is None:
        app = QApplication(sys.argv)
    myWindow = WindowClass()
    myWindow.show()
    sys.exit(app.exec_())
```
