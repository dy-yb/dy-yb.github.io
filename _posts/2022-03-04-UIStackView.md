---
title: UIStackView
date: 2022-03-04 23:00:00 +0900
tags: swift study UIStackView stackview
---

### UIStackView란?

열 또는 행에 여러 View를 묶어서 배치할 수 있는 인터페이스로, 오토레이아웃 제약조건을 설정하지 않아도 자동으로 사이즈에 맞게 View의 사이즈나 위치를 정렬 해준다.

스택뷰는 다음 속성들을 통해 view를 정렬해준다.

- **Axis(축)**
    - 스택 뷰의 축의 방향에 따라 뷰를 배열한다.
    
    ![스크린샷_2022-03-04_22 46 07](https://user-images.githubusercontent.com/40792935/156783888-39cd1668-623e-476c-acdf-621fd820304d.png)
    
    - Vertical: 탑을 쌓듯이 위에서 아래로
    - Horizontal: 책꽂이 처럼 왼쪽에서 오른쪽으로
    
- **Spacing(여백)**
    - 뷰들이 가져야할 최소한의 여백을 설정
    - 만약 여백과 뷰의 사이즈를 합한 것이 스택뷰 사이즈를 넘어가게 된다면 뷰들의 사이즈를 감소
    
- **Distribution(분배)**
    - 스택 뷰의 축을 따라 내부 뷰들의 사이즈를 어떻게 분배하여 갖게 할 것인가에 대한 옵션
    - 뷰들의 사이즈의 합이 스택 뷰의 사이즈를 초과할 경우에는 Compression resistance priority에 따라 뷰의 크기를 감소
    - 뷰들의 크기가 스택 뷰의 사이즈보다 부족할 때에는 Content hugging priority에 따라 뷰의 크기를 증가
    - Content hugging priority
        - 뷰에 우선순위를 설정하여 스택 뷰 전체 사이즈가 변화함에 따라 
        **낮은 우선순위**를 가진 뷰는 **크기가 커지고**, **높은 우선순위**를 가진 뷰는 본래 **크기 유지**
    - Compression resistance priority
        - **낮은 우선순위**를 가진 뷰는 **크기가 작아지고**, **우선순위가 높을수록** 본래 **크기 유지**
    - Content hugging priority, Compression resistance priority는 0~1000까지의 숫자로 설정 가능하며 1000이 가장 높은 우선순위
    - Options
        - Fill
            - 스택 뷰의 축을 따라 가능한 모든 공간을 채우기 위해 내부 뷰들의 사이즈를 조정
                
                <img width="687" alt="스크린샷_2022-03-04_22 49 36" src="https://user-images.githubusercontent.com/40792935/156782750-85b2cd89-7086-4680-987b-eee588cd060e.png">
                
        - Fill Equally
            - 뷰들이 최대한 같은 크기로 분배하여 공간을 채우도록 하는 옵션
                
                <img width="685" alt="스크린샷_2022-03-04_22 49 45" src="https://user-images.githubusercontent.com/40792935/156782757-e556df31-e433-4799-a6ad-a17e3ce9fdc6.png">
                
        - Fill Proportionally
            - intrinsic content size 비율에 따라 뷰들의 사이즈를 조정
                
                <img width="719" alt="스크린샷_2022-03-04_23 18 03" src="https://user-images.githubusercontent.com/40792935/156782773-80031c79-d01a-4659-9ccb-f93b8cc774d6.png">
                
        - Equal Spacing
            - 스택뷰의 공간이 남을 경우 뷰들의 사이의 공간을 동일하게 재배치하고, 공간이 부족하면 리사이징을 통해 뷰를 줄여서라도 공간이 동일하도록 함
                
                <img width="727" alt="스크린샷_2022-03-04_23 16 42" src="https://user-images.githubusercontent.com/40792935/156783653-96d0e57a-5156-43a9-b17b-e862dcbedc47.png">
                
        - Equal Centering
            - 각 뷰들의 center와 center간 거리를 동일하게 하도록 재배치
                
                <img width="739" alt="스크린샷_2022-03-04_23 16 52" src="https://user-images.githubusercontent.com/40792935/156782768-6c2d554b-22ea-4272-a100-85767e1910b1.png">
                
            - 이 때 spacing 값 만큼의 거리는 반드시 유지되어야 하며, 만약 뷰들이 스택뷰의 사이즈를 초과할 경우 spacing의 거리를 만족시킬 때까지 간격을 좁히고, 그래도 초과하면 뷰의 사이즈를 조정

- **Alignment(정렬)**
    - 정렬 방식을 어떻게 할 것인가?
    - Options
        - Fill
            - 축의 수직인 방향으로 공간을 채우기 위해 뷰들의 사이즈를 조정
                - ex) 축이 가로로 되어있다면 뷰의 세로 길이(height)를 늘림
                
                <img width="719" alt="스크린샷_2022-03-04_23 23 23" src="https://user-images.githubusercontent.com/40792935/156782776-ef3374fa-f1a4-4572-ad6e-198175285bf6.png">                
            
        - Leading
            - 스택뷰와 내부 뷰의 leading edge를 맞추어 정렬
                
                ![Untitled](https://user-images.githubusercontent.com/40792935/156782799-655e7131-460d-456b-bbe7-4166d9f86b7b.png)
                
                - 세로축의 경우 왼쪽 혹은 오른쪽이 되고
                - 가로축의 경우에는 top과 동일한 결과가 나옴
        - Top
            - 스택 뷰와 내부 뷰들의 top edge를 맞추어 정렬
                
                <img width="737" alt="스크린샷_2022-03-04_23 27 07" src="https://user-images.githubusercontent.com/40792935/156782779-dd0298b2-60fc-4e82-99b0-26c96b60f058.png">
                
        - FirstBaseline
            - 뷰들의 first baseline에 맞추어 정렬
            
            ![Untitled 1](https://user-images.githubusercontent.com/40792935/156782782-6cca5131-dc1f-4321-89be-8921ce7c3003.png)
            
            - 가로축을 가진 스택뷰에서만 사용 가능
        - Center
            - 축을 따라 뷰들의 center를 스택뷰의 center에 맞추어 조정
            
            ![Untitled 2](https://user-images.githubusercontent.com/40792935/156782784-30f319f6-f5e9-4e52-a988-7d651fe0e617.png)
            
        - Trailing
            - 스택 뷰와 내부 뷰들의 trailing edge를 맞추어 정렬
                
            ![Untitled 3](https://user-images.githubusercontent.com/40792935/156782789-d40add40-2765-41b6-af52-36190010d6bd.png)
                
        - Bottom
            - 스택 뷰와 내부 뷰들의 bottom edge를 맞추어 정렬
            
            ![Untitled 4](https://user-images.githubusercontent.com/40792935/156782793-d946eabb-3d49-43bd-9648-cc1a788c773d.png)
            
        - LastBaseline
            - 뷰들의 first baseline에 맞추어 정렬
                
            ![Untitled 5](https://user-images.githubusercontent.com/40792935/156782796-2829c541-1dda-4334-8437-87d720bb5dc4.png)
                
            - 가로축을 가진 스택뷰에서만 사용 가능



### Reference
https://hyunndyblog.tistory.com/148
