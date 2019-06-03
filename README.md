# 제주도 장소 검색하기

|학과 | 학번 | 성명|
|수학과 | 201711142 | 이예림 |
|본인학과 |본인학번 |본인학성명|


## 프로젝트 개요
본인의 프로젝트 개요에 대하여 작성합니다.

## 사용한 공공데이터 
[데이터보기](https://www.data.go.kr/dataset/15004770/fileData.do)

## 소스
* [링크로 소스 내용 보기](https://www.youtube.com/watch?v=Ix2IiLX6mS0) 

* 코드 삽입
~~~python
import os

nDivCnt=1001

filepath='C:/Users/lee/Desktop/python/'

fileName='제주특별자치도'

fileExe='.csv'

dirname=filepath
if not os.path.isdir(dirname) :
    os.mkdir(dirname)

nLineCnt=0
nFileIdx=0
f=open('제주특별자치도.csv','r')
fDivName=open('%s%d%s'%(filepath+fileName,nFileIdx,fileExe),'w')

fline=f.readline()
fDivName.write(fline)
while True :
    line=f.readline()
    if not line : break

    if nLineCnt==nDivCnt :
        fDivName.close()
        nFileIdx+=1
        nLineCnt=0
        strPat='%s%d%s'%(filepath+fileName,nFileIdx,fileExe)
        fDivName=open(strPat,'w')
        nLineCnt+=1
        fDivName.write(fline)
        print("생성 완료 ",strPat)

    nLineCnt+=1
    fDivName.write(line)

fDivName.close()
f.close()
~~~
~~~python
import pandas as pd

cc=input('파일번호')
df=pd.read_csv("제주특별자치도"+cc+".csv",encoding='cp949')

ch=input('전화번호? 주소?')
if ch == '전화번호' :
    #장소검색
    #장소 명칭 열을 리스트로 만든다
    sp=list(df['장소 명칭'])

    #사용자에게 입력을 받는다
    c=input('장소를 검색하세요')

    #리스트 요소 중 사용자에게 입력받은 문자열을 포함한 요소가 있다면
    #j 번째 행의 장소 명칭과 전화번호를 출력한다
    j=0
    for i in sp :
        if c in i :
            print(i)

            p=list(df.loc[j])
            print(p[5])
        j+=1

elif ch == '주소':
    #주소검색
    tp=list(set(list(df['종류 구분 체계 '])))

    for i in range(len(tp)) :
        print(i+1,tp[i])

    c1=int(input('종류 구분 체계를 선택해주세요'))-1
    p1=df[(df['종류 구분 체계 ']==tp[c1])]

    print('\n',c1+1,tp[c1])
    print(p1)

    ad1=list( df['주소'] )
    #ad2=list( df[ (df['신규 거리명 주소']) & (df['종류 구분 체계 ']==tp[c1]) ] )

    j=0
    for i in ad1 :
        #i를 띄어쓰기를 기준으로 나눠서 리스트를 만들고
        i=i.split(' ')
        #이 리스트들을 요소로 갖는 리스트가 ad1이 된다
        ad1[j]=i
        j+=1

    #사용자가 '제주시'를 입력하면 제주시문자열출력
    c2=input('제주시/서귀포시')
    if c2=='제주시' :
        p2='일도1동, 일도2동, 이도1동, 이도2동, 삼도1동, 삼도2동, 건입동, 용담1동, 용담2동, 용담3동, 화북1동, 화북2동, 삼양1동, 삼양2동, 삼양3동, 봉개동, 아라1동, 아라2동, 오라1동, 오라2동, 오라3동, 노형동, 외도1동, 외도2동, 이호1동, 이호2동, 도두1동, 도두2동, 도남동, 도련1동, 도련2동, 용강동, 회천동, 오등동, 월평동, 영평동, 연동, 도평동, 해안동, 내도동, 한림읍, 애월읍, 구좌읍, 조천읍, 한경면, 추자면, 우도면, 화북동, 삼양동, 아라동, 오라동, 외도동, 이호동, 도두동'
    else :
        p2=='서귀동, 법환동, 서호동, 호근동, 동홍동, 서홍동, 상효동, 하효동, 신효동, 보목동, 토평동, 중문동, 회수동, 대포동, 월평동, 강정동, 도순동, 하원동, 색달동, 상예동, 하예동, 영남동, 대정읍, 남원읍, 성산읍, 안덕면, 표선면, 송산동, 정방동, 중앙동, 천지동, 효돈동, 영천동, 대륜동, 대천동, 예래동'
    print(c2,'\n',p2)

    #사용자가 '일도1동'을 입력하면 그에 해당하는 곳들 출력
    c3=list(input('동/읍/면'))
    if c3[2] == '1' : c3[2]='일'
    elif c3[2]=='2' : c3[2]='이'
    elif c3[2]=='3' : c3[2]='삼'
    c3=''.join(c3)

    for l in range(len(ad1)) :
        if ad1[l][2]==c3 :
            p3=df.loc[l]
            if p3['종류 구분 체계 ']==tp[c1] :
                print(p3[['장소 명칭','주소','신규 거리명 주소']])
    #도로명 주소는 없는 장소 목록도 있어서 생략
    #만약 만들고 싶다면 주소와 같은 방법으로 코딩
~~~
