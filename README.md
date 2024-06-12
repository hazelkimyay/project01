# project01
ICT_파일럿 프로젝트

```
### 키워드 추출 및 분류

!pip install konlpy

import pandas as pd
data=pd.read_csv('/content/AI_240610.csv')
data
# 필요라이브러리 import
- pandas: 데이터프레임 조작을 위해 사용
- TfidfVectorizer: TF-IDF 벡터화를 위해 사용
- Okt: 한국어 형태소 분석을 위해 사용
- re: 정규 표현식을 사용하기 위해
### WHY **TF-IDF**?)
- TF-IDF 는 문서내에서  해당단어(term)이 얼마나 등장했는지 세서 단어의 중요도를 평가하는 통계적 측정방법입니다.
- 중요한 단어에는 높은 가중치, 중요하지 않은 단어에는 낮은 가중치를 부여합니다.
### what **Okt**?)

- 한국어 자연어 처리를 위한 형태소 분석기입니다.
- 형태소분석, 품사태깅,명사추출, 토큰화, 정규화, 어간추출등의 기능이 있습니다.
import pandas as pd
from sklearn.feature_extraction.text import TfidfVectorizer
from konlpy.tag import Okt
import re

# 불용어 로드
def load_stopwords(filepath):
    with open(filepath, 'r', encoding='utf-8') as file:
        stopwords = file.read().splitlines()
    return stopwords

stopwords = load_stopwords('stopword.txt')

#데이터 프레임 생성
df = pd.DataFrame(data)

# 데이터 전처리
# 특수 문자 제거, 소문자로 변환 등
df['content'] = df['content'].apply(lambda x: re.sub(r'\n', ' ', x))  # 줄바꿈 제거
df['content'] = df['content'].apply(lambda x: re.sub(r'[^가-힣\s]', '', x))  # 한글 및 공백 제외 제거
df['content'] = df['content'].apply(lambda x: x.lower())  # 소문자 변환 (한국어는 필요 없지만 일관성 유지)

# 형태소 분석기 초기화
okt = Okt()

#텍스트 전처리 함
# 형태소 분석 및 불용어 제거
def preprocess_text(text):
    tokens = okt.morphs(text, stem=True)  # 형태소 분석 및 어간 추출
    tokens = [word for word in tokens if word not in stopwords]
    return ' '.join(tokens)

df['content'] = df['content'].apply(preprocess_text)

# TF-IDF 벡터
tfidf = TfidfVectorizer()
tfidf_matrix = tfidf.fit_transform(df['content'])

# TF-IDF 결과를 데이터프레임으로 변환
tfidf_df = pd.DataFrame(tfidf_matrix.toarray(), columns=tfidf.get_feature_names_out())

# 각 기사별 상위 n개 키워드 추출
n = 5  # 상위 5개 키워드 추출
top_n_keywords = {}

for i, row in tfidf_df.iterrows():
    top_keywords = row.sort_values(ascending=False).head(n).index.tolist()
    top_n_keywords[df['title'][i]] = top_keywords

# 결과 출력
for title, keywords in top_n_keywords.items():
    print(f"Title: {title}\nTop Keywords: {keywords}\n")

결과)

- 상위 n개의 키워드를 추출합니다.
- 각 기사별로 TF-IDF 값이 높은 상위 n개의 단어를 추출합니다.
- 기사 제목과 상위 키워드를 딕셔너리에 저장합니다.
해석)
- Title **"[단독] AI·사이버 보안... 수도권 대학 ‘첨단 학과’ 569명 늘린다"** 로 부터

Top Keywords: **['정원', '학과', '대학', '수도권', '증원']** 키워드 추출됨
```




```
### 웹페이지 UIUX 구현 코드

import streamlit as st

# Set the page configuration
st.set_page_config(page_title="News Keywords", page_icon=":newspaper:")

# Custom CSS for styling
st.markdown("""
    <style>
        .header {
            text-align: center;
            background: linear-gradient(to right, #d4fc79, #96e6a1);
            -webkit-background-clip: text;
            color: transparent;
            font-size: 4em;
            font-weight: bold;
            margin-bottom: 20px;  /* Reduced margin */
        }
        .categories {
            display: flex;
            justify-content: center;
            gap: 10px;
            margin-bottom: 50px;
            white-space: nowrap;  /* Prevent text wrapping */
        }
        .category-button {
            padding: 10px 20px;
            border-radius: 20px;
            background-color: #f0f0f0;
            border: none;
            cursor: pointer;
            transition: background 0.3s;
        }
        .category-button:hover {
            background-color: #ddd;
        }
        .main-description {
            text-align: center;
            margin-bottom: 50px;
        }
        .search-container {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 10px;
        }
        .search-input {
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 20px 0 0 20px;
            outline: none;
            width: 300px;
        }
        .search-button {
            padding: 10px;
            border: none;
            border-radius: 0 20px 20px 0;
            background: #1e90ff;
            color: white;
            cursor: pointer;
            outline: none;
        }
    </style>
""", unsafe_allow_html=True)

# Render the header
st.markdown("<div class='header'>NEWS KEYWORDS</div>", unsafe_allow_html=True)

# Render the category buttons in a single row without wrapping
st.markdown("<div class='categories'>", unsafe_allow_html=True)
categories = ["검색하기", "경제", "정치", "생활/문화", "IT/과학", "세계", "스포츠", "연예"]
cols = st.columns(len(categories))

for col, category in zip(cols, categories):
    with col:
        if st.button(category, key=category):
            st.write(f"Selected category: {category}")
st.markdown("</div>", unsafe_allow_html=True)

# Render the main description
st.markdown("""
    <div class='main-description'>
        <h3>시사 키워드 리스트 서비스</h3>
        <p>키워드로 알아보는 최신 핵심 뉴스</p>
    </div>
""", unsafe_allow_html=True)

# Render the search bar
st.markdown("""
    <div class='search-container'>
        <input class='search-input' type='text' placeholder='관심 이슈 및 키워드를 입력하세요'>
        <button class='search-button'>검색</button>
    </div>
""", unsafe_allow_html=True)
```

