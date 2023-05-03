# FFT (Fast Fourier Transform)
**이산 푸리에 변환(Discrete Fourier Transform, DFT)** 의 수식을 이용해서, 실제 신호에 대한 
푸리에 변환을 하기에는 계산량이 너무 많다. DFT의 시간 복잡도는 O($N^2$)으로, 아래의 수식을 
따르면 곱 연산을 $N^2$회, 덧셈 연산은 N(N-1)회 수행해야 한다.

<img width="238" alt="dft" src="https://user-images.githubusercontent.com/127461144/235883098-aa115555-5f5a-4eda-8ea5-dc5e893b8724.png">


**고속 푸리에 변환(Fast Fourier Transform, FFT)** 은 주기성과 대칭성을 이용하여 DFT의 계산량을 줄이는 알고리즘으로, 시간 복잡도는 O($logN$)이다. 

<img src="https://user-images.githubusercontent.com/127461144/235883474-70b59ecb-6de8-4111-91b8-8db21d6d5b87.jpg"  width="300" height="300">


FFT는 신호를 개별 스펙트럼 구성 요소로 변환하여 신호에 대한 주파수 정보를 제공한다. 오디오 및 음향 측정 과학 분야에 사용되며, 기계 또는 시스템 결함 분석, 품질 관리 및 상태 모니터링에 사용된다.

FFT를 구현하는 대표적인 알고리즘에는 쿨리-튜키 알고리즘(Cooley-Tukey algorithm)이 있다.


## FFT 시간 복잡도 계산
쿨리-튜키 알고리즘(Cooley-Tukey algorithm)을 통해 DFT를 고속 계산할 것이다.
쿨리-튜키 알고리즘(Cooley-Tukey algorithm) 은 분할 정복 알고리즘의 유형이다.

분할 정복 알고리즘이란 주어진 문제의 입력을 분할하여 문제를 해결하는 방식의 알고리즘을 말한다. 분할한 입력에 대하여 동일한 알고리즘을 적용하여 해를 얻으며, 이들의 해를 취합하여 원래의 해를 얻는다. 

DFT의 식을 분할 정복 형태로 만들어 보자.
먼저 N개의 데이터 을 짝수, 홀수 2개로 나눈다.

<img width="158" alt="짝홀" src="https://user-images.githubusercontent.com/127461144/235886057-5c7fb4b7-2b67-4207-a620-4dcb890cd375.png">


그 다음 DFT의 식에 대입해보자.

<img width="413" alt="분할" src="https://user-images.githubusercontent.com/127461144/235886106-724367f3-cf7c-4ccb-b703-b930e2c3562d.png">


DFT를 짝수항 DFT와 상수값이 곱해진 홀수항 DFT의 합의 형태로 나타낼 수 있다. 식을 정리해보면 아래와 같이 나타낼 수 있다.

<img width="281" alt="최종" src="https://user-images.githubusercontent.com/127461144/235886560-dbe4ad24-2cfa-49c3-b454-f78cf1b02ada.png">

이와 같이 N개의 데이터를 가진 DFT의 식을, $\frac{N}{2}$ 개의 두 DFT 식으로 표현해, 분할 정복의 형태로 변형시켰다. 따라서 두 DFT를 알고 있다면 k번의 연산만에 구할 수 있고, 총 시간복잡도는 O($logN$)이 된다.







