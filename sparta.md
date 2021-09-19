# 힙한취미코딩

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


추석연휴동안 파이썬 크롤링 코딩을 연습하기 위해서 알아보던중
스파르타코딩클럽에서 이벤트를 하고 있어서 참가하게되었는데
기존에 배웠던 것들도 리마인드되고
강사님이 아주 친절하게 차근차근 잘 설명해주셔서
잘 이해를 못했었던 개념들도 완전히 이해할 수가 있었다.
위에 코드는 1주차 숙제 코드인데 앞으로 남은 과제들도
남은 추석연휴동안 열심히 공부할 예정이다.
