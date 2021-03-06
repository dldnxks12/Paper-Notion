## Markov Chain

[참고 링크 1](https://sites.google.com/site/machlearnwiki/RBM/markov-chain) <br>
[참고 링크 2](https://www.puzzledata.com/blog190423/)

- Markov Chain

        Markov Property를 가진 이산 확률과정 모델을 뜻한다. 

          * Markov Property : 특정 상태의 확률은 오직 특정 기간의 과거 상태에 의존한다.

            Ex) 오늘 날씨가 맑다면 내일 날씨는 맑을 지, 비가 내릴 지.. 

       
- n 차 Markov Chain

        N+1 번 째 상태가 N번 째 상태에만 영향을 받는다면 1차 Markov Chain 

        N+1 번 째 상태가 N번 째, N-1 상태에 영향을 받는다면 2차 Markov Chain 


<div align=center>
  
![image](https://user-images.githubusercontent.com/59076451/131969359-5f5ea585-ee0c-4617-afef-763645cf3238.png)
  
</div>  

---

<br>

#### Why Markov Chain?

    Markov Chain model을 이용해서 확률이 제일 크게 되는 쪽으로 미래를 예측해볼 수 있다!

<br>

#### What can we do?

- 풀수 있는 문제 

<br>

  **문제 1)** 과거의 관측치가 아래와 같을 때, 다음 날의 날씨는 무엇일까?

      Sunny - Sunny - Cloudy - Rainy - Sunny - ?

1차 Markov Chain 모델이라 가정하면 위의 문제는 직전 State만을 이용한 P(? | Sunny) 중 가장 큰 확률의 값을 취한다.

      Sol : 만약 P( Sunny | Sunny )  = 0.7 , P( Cloudy | Sunny )  = 0.1 , P( Rainy | Sunny )  = 0.2

            라면, 다음 날의 날씨는 Sunny로 예측한다. 

<br>

하지만 우리는 단순히 과거의 정보들로 오늘 날씨를 예측하고 싶은 것이 아니다.

**우리는 이 정보들을 통해 미래를 예측해보고 싶은 것이다!**

다음의 문제를 보자 

<br>

  **문제 2)** 오늘 날씨가 Sunny인 경우, 다음 7일 간의 날씨 변화가 아래와 같을 확률은?

    sunny - sunny - rainy - rainy - sunny - cloudy - sunny

이 문제는 다음과 같이 Sequence Probability 를 이용해서 계산할 수 있다. 

    Sol :  P(q1, q2, ...., qt)

           = P(q1) x P(q2 | q1) x P(q3 | q1, q2) x ... x P(qt-1 | q1, q2,...qt-2) x P(qt | q1, q2,...qt-1)  
                  ... P(qt | q1, q2,...qt-1) <- chain rule
                
           = P(q1) x P(q2 | q1) x P(q3 | q2) x ... x P(qt-1 | qt-2) x P(qt | qt-1)
                  ....Markov chain  ... ( if 1th order )

결과적으로 Chain rule에 따라 복잡하게 계산해야 구할 수 있던 Sequence에 대한 확률을 Markov chain으로 쉽게 구할 수 있다.

    물론 모델을 train 시켜 상태 천이 확률이 수렴된 후에 사용할 수 있지만 말이다!
      

---

<br>

#### 상태 전이 확률

<div align=center>

Markov Chain를 충분히 학습시켰다고 가정하면, 특정 상태에서 다음 상태로 이동할 전이 확률은 특정 값에 수렴하게 된다.   

Graph로 표현하게 되면 아래와 같다.
  
![image](https://user-images.githubusercontent.com/59076451/131969764-24545a43-eaa9-4f4b-92e6-aa7d4fa32111.png)
  
아래 식은 상태 전이 확률을 식으로 나타낸 것이며, 그 아래의 식은 상태 전이 확률의 조건이다. 
  
![image](https://user-images.githubusercontent.com/59076451/131970399-553b6943-b81b-406a-8d87-846c4c43598c.png)
  
  
</div>  

      Markov Chain 에서는 상태 전이 확률이 핵심이 되고, 위와 같은 상태 전이도로 표현하여 시각화한다. 


---

<br>

#### Markov Chain 예시


1차 Markov Chain을 이용한 예시를 들어보자.

날씨 영역은 두 가지 상태를 나타낸다. ( S = {맑음, 비} )

그리고 날씨는 다음과 같이 상태 전이 확률표로 주어진다. (때로는 상태 전이도로 주어진다.)

      날씨의 천이 확률표는 다음과 같이 계산되어 나타낸 진 것이다.
      
      P( N 번째 상태 : 맑음| N-1 번째 상태 = 비)
               
<div align=center>


**상태 천이표**  
  
![image](https://user-images.githubusercontent.com/59076451/131971585-14f60156-f84f-47f2-a1d2-7c6c88f030f1.png)

<br>
  
<br>  
  
**상태 천이도**  
  
![image](https://user-images.githubusercontent.com/59076451/131971604-ab1723c2-f23e-41ad-a686-23a5b37e59d6.png)
 
![image](https://user-images.githubusercontent.com/59076451/131971699-857998a4-7a19-4343-bb9d-52c9440e751c.png)

위의 상태 천이도는 아래와 같이 행렬로 표현할 수 있다.   
  
![image](https://user-images.githubusercontent.com/59076451/131971736-aa62a9c8-af6f-4d11-90bd-969f50e001e2.png)
  
또한 아래와 같이 행렬곱을 통해 다음날, 그 다음날, ... 에 대한 예측을 수행할 수 있다.   

![image](https://user-images.githubusercontent.com/59076451/131971854-221e45b9-0d15-4df6-9537-d08178e515fb.png)
  
</div>  


위의 표를 이용해서 모레의 날씨를 추정해보자.

      만일 5월 10일이 지난 3년간 80%의 확률도 맑았다면, 내년 5월 10일도 80%확률도 맑을 것이다.
      
      그럼 5월 10일 기준으로 5월 12일의 날씨가 흐릴 확률은 어떨까? 
      
      ( 5월 10일의 날씨가 맑을 확률   X   5월 12일의 날씨가 맑은 확률 ) + ( 5월 10일의 날씨가 흐릴 확률   X   5월 12일의 날씨가 맑은 확률  )
      
      = (0.8 x 0.565) + (0.2 x 0.362)  = 0.524
      
      즉 , 52 %의 확률로 맑을 것이란 예측이 나온다.
      
     



