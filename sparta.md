# 힙한취미코딩
추석연휴동안 파이썬 크롤링 코딩을 연습하기 위해서 알아보던중
스파르타코딩클럽에서 이벤트를 하고 있어서 참가하게되었는데
기존에 배웠던 것들도 리마인드되고
강사님이 아주 친절하게 차근차근 잘 설명해주셔서
잘 이해를 못했었던 개념들도 완전히 이해할 수가 있었다.
첫번째 과제는 이미지를 크롤링하는 웹스크래핑이었는데
내가 좋아하는 스타들의 사진을 크롤링하여
추후에 작업할때 바로 써먹게끔 정리하는데 유용할것 같다.
두번째 과제는 네이버 기사 크롤링이었는데
이 강의가 촬영된게 작년 2020년 추석때였어서
아무래도 현재 2021년의 네이버 HTML구조?가 바뀌어서
미션은 실패했지만 추후에 코드를 수정볼 예정이다.
세번째 과제는 카톡방 대화를 워드클라우드하는 거였는데
아무래도 친구들과의 단톡방이 많다보니까
오랜시간동안 친구들과 나누었던 데이터가 많이 쌓여있어서
무척이나 재미있었다.
추후에 데이터 클렌징을 좀 더 공부해서
내가 얻고 싶은 데이터를 볼 수 있게끔 공부해봐야겠다.

스파르타코딩클럽 추석 이벤트 참여하러가기
https://spartacodingclub.kr/?f_name=%EA%B9%80%EC%84%B8%ED%99%98&f_uid=6139b57d2dfe198f0c6aa773

[스파르타코딩클럽] 파이썬 혼자놀기 패키지 노션 강의자료
https://www.notion.so/0cbbfc077f87439bb2bd6657b1dd12aa

## 1일차 과제

    from bs4 import BeautifulSoup
    from selenium import webdriver
    import time
    import dload

    driver = webdriver.Chrome('chromedriver')
    driver.get("https://search.daum.net/search?nil_suggest=btn&w=img&DA=SBC&q=%EC%97%90%EC%8A%A4%ED%8C%8C+%EC%B9%B4%EB%A6%AC%EB%82%98")
    time.sleep(5)

    req = driver.page_source
    soup = BeautifulSoup(req, 'html.parser')

    thumbnails = soup.select('#imgList > div > a > img')

    i = 1
    for thumbnail in thumbnails:
        img = thumbnail['src']
        dload.save(img, f'imgs_homework/{i}.jpg')
        i += 1

    driver.quit()

## 2일차 과제

    from openpyxl import Workbook
    from bs4 import BeautifulSoup
    from selenium import webdriver

    driver = webdriver.Chrome('chromedriver')
    url = "https://search.naver.com/search.naver?where=news&sm=tab_jum&query=추석"
    driver.get(url)
    req = driver.page_source
    soup = BeautifulSoup(req, 'html.parser')

    articles = soup.select('#main_pack > section.sc_new.sp_nnews._prs_nws > div > div.group_news > ul > li')
    wb = Workbook()
    ws1 = wb.active
    ws1.title = "articles"
    ws1.append(["제목", "링크", "신문사", "썸네일"])

    for article in articles:
        title = article.select_one('div > div > a').text
        url = article.select_one('div > div > a')['href']
        comp = article.select_one('a.info.press').text.split(' ')[0].replace('언론사', '')
        thumbnail = article.select_one('div > a')['href']
        ws1.append([title, url, comp, thumbnail])

    driver.quit()
    wb.save(filename='articles.xlsx')



## 3일차 과제

    from wordcloud import WordCloud
    from PIL import Image
    import numpy as np

    text = ''
    with open("20210919_장태산_대화.txt", "r", encoding="utf-8") as f:
        lines = f.readlines()
        for line in lines:
            if '] [' in line:
                text += line.split('] ')[2].replace('그럼', '').replace('근데', '').replace('나도', '').replace('나', '').replace('난', '').replace('아', '').replace('ㅋ', '').replace('ㅠ', '').replace('ㅜ', '').replace('이모티콘\n', '').replace('사진\n', '').replace('삭제된 메시지입니다.\n', '')

    mask = np.array(Image.open('cloud.png'))
    wc = WordCloud(font_path='C:/WINDOWS/Fonts/malgun.ttf', background_color="white", mask=mask)
    wc.generate(text)
    wc.to_file("result_masked.png")
