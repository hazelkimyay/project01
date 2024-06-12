# project01
ICT_파일럿 프로젝트






```
# 웹페이지 UIUX 구현 코드

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

