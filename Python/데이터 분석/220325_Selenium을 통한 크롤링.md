### Selenium을 통한 크롤링
	- requests로 요청을 해도 막아놓은 사이트가 많음
	- Selenium은 웹앱을 테스트하는데 사용하는 프레임워크
	- webdriver라는 API를 통해 운영체제에 설치된 브라우저(Chrome, Edge 등)을 제어

1. webdriver 설치
	- 크롬: https://chromedriver.chromium.org/downloads
	- Edge: https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver/
	- 파이어폭스:https://github.com/mozilla/geckodriver/releases

	
	
2. selenium 설치
	
	- !pip install selenium
	
	  
	
3. 모듈 호출

    ```python
    from selenium import webdriver
    from bs4 import BeautifulSoup as BS
    import time
    import pandas as pd
    ```
    
    
    
4. webdriver 연결

    ```python
        driver=webdriver.Chrome("./chromedriver.exe")
    ```

    

5. url 연결(네이버 블로그 검색 기준)

    ```python
    url="https://section.blog.naver.com/Search/Post.naver?pageNo=1&rangeType=ALL&orderBy=sim&keyword=국내 여행지"
    driver.get(url)
    
    ```

    

6. 데이터 가져오기

    ```python
    html=driver.page_source
    soup=BS(html, "html.parser")

    ```

7. 웹에서 개발자 모드(F12)로 원하는 데이터의 위치를 파악.

   - find 메서드 : html 태그를 이용해서 사용
     - 지정된 키워드는 언더바를 넣어서 사용 ( class --> class_ )
   - select메서드 사용시: 개발자 모드에서 우클릭 > 복사 > selector복사

   ```python
   post_soup=soup.find_all("div", class_="list_search_post")
   ```

   

8. for문 사용해서 사용할 데이터를 추출
   
   ```python
   post_list=[]
   for post in post_soup:
       title=post.find("strong",class_="title_post").get_text()
       href=post.find("a")["href"]
       post_list.append({"제목":title, "링크":href})
   ```
   
   
   
9. DataFrame형태로 변경
   
   ```python
    df=pd.DataFrame(post_list)
    df
   ```
   
   



