# FFT (Fast Fourier Transform)
**이산 푸리에 변환(Discrete Fourier Transform, DFT)** 의 수식을 이용해서, 실제 신호에 대한 
푸리에 변환을 하기에는 계산량이 너무 많다. DFT의 시간 복잡도는 O($N^2$)으로, 아래의 수식을 
따르면 곱 연산을 $N^2$회, 덧셈 연산은 N(N-1)회 수행해야 한다.

<img width="238" alt="dft" src="https://user-images.githubusercontent.com/127461144/235883098-aa115555-5f5a-4eda-8ea5-dc5e893b8724.png">


**고속 푸리에 변환(Fast Fourier Transform, FFT)** 은 주기성과 대칭성을 이용하여 DFT의 계산량을 줄이는 알고리즘으로, 시간 복잡도는 O($NlogN$)이다. 

<img src="https://user-images.githubusercontent.com/127461144/235883474-70b59ecb-6de8-4111-91b8-8db21d6d5b87.jpg"  width="300" height="300">


FFT는 신호를 개별 스펙트럼 구성 요소로 변환하여 신호에 대한 주파수 정보를 제공한다. 오디오 및 음향 측정 과학 분야에 사용되며, 기계 또는 시스템 결함 분석, 품질 관리 및 상태 모니터링에 사용된다.

FFT를 구현하는 대표적인 알고리즘에는 쿨리-튜키 알고리즘(Cooley-Tukey algorithm)이 있다.


# FFT 시간 복잡도 계산
쿨리-튜키 알고리즘(Cooley-Tukey algorithm)은 분할 정복 알고리즘의 유형으로, 이를 통해 DFT를 고속 계산할 것이다.

분할 정복 알고리즘이란 주어진 문제의 입력을 분할하여 문제를 해결하는 방식의 알고리즘을 말한다. 분할한 입력에 대하여 동일한 알고리즘을 적용하여 해를 얻으며, 이들의 해를 취합하여 원래의 해를 얻는다. 

DFT의 식을 분할 정복 형태로 만들어 보자.
먼저 N개의 데이터 을 짝수, 홀수 2개로 나눈다.

<img width="158" alt="짝홀" src="https://user-images.githubusercontent.com/127461144/235886057-5c7fb4b7-2b67-4207-a620-4dcb890cd375.png">


그 다음 DFT의 식에 대입해보자.

<img width="413" alt="분할" src="https://user-images.githubusercontent.com/127461144/235886106-724367f3-cf7c-4ccb-b703-b930e2c3562d.png">


DFT를 짝수항 DFT와 상수값이 곱해진 홀수항 DFT의 합의 형태로 나타낼 수 있다. 식을 정리해보면 아래와 같이 나타낼 수 있다.

<img width="281" alt="최종" src="https://user-images.githubusercontent.com/127461144/235886560-dbe4ad24-2cfa-49c3-b454-f78cf1b02ada.png">

이와 같이 N개의 데이터를 가진 DFT의 식을, $\frac{N}{2}$ 개의 두 DFT 식으로 표현해, 분할 정복의 형태로 변형시켰다. 

<img width="86" alt="복소수" src="https://user-images.githubusercontent.com/127461144/236095041-b82f1e34-80f5-4acd-bccf-972583d0cf4f.png">
로 설정하고, k = 0부터 살펴보자  

 <img width="266" alt="X(0)" src="https://user-images.githubusercontent.com/127461144/236096232-92ab206c-d1bf-4fae-aceb-7425d9e43fa9.png">    

위의 식에서 총 N번의 곱셈 연산과 덧셈의 연산이 일어난다. 시간 복잡도는 O(N)으로 표현한다. 즉, N개의 원소에 대해서 2로 나눴을 때 연산량이 N회 발생한다.

다음으로 $X_even$과 $X_odd$를 구하기 위해서, 각각 반으로 나눈다. $X_even$를 A와 B로 나누고, $X_odd$를 C와 D로 나누어 보자.  

<img width="268" alt="짝홀 나누기" src="https://user-images.githubusercontent.com/127461144/236097603-b8a77ec6-f0f9-492e-8d65-ea14db3c7033.png">  

짝수항과 홀수항을 구하기 위해서도 N번의 연산이 필요하다. 이러한 과정을 원소가 2개가 될 때까지 반복하고, 마지막으로 원소가 2개 남았을 때에도 N번의 연산이 필요하다. 따라서 O($NlogN$)이 된다. 

마지막으로 정리해보면 다음과 같은 식을 얻을 수 있다.  
<img width="293" alt="최종식" src="https://user-images.githubusercontent.com/127461144/236099646-2a73287d-8bf7-4353-bda1-62c7daca00f1.png">  

# FFT구현
쿨리-튜키 알고리즘으로 구현할 수 있다. 재귀적으로 절반의 크기를 구하기 때문에 기본적으로 수열의 길이를 $2^N$으로 설정한다. 짝수항 DFT와 홀수항 DFT을 구해서 연산을 하면 계속 이에 해당하는 메모리를 할당해야 하는 문제점이 있다. 

$X = \{X_0, X_1, X_2, X_3, X_4, X_5, X_6, X_7\}$  
$X_even = \{X_0, X_2, X_4, X_6\}$  
$X_odd = \{X_1, X_3, X_5, X_7\}$  

위와 같이 $X$를 설정하면 $X_even$과 $X_odd$의 메모리를 할당해서 구할 것이고, 또 재귀적으로 메모리 할당이 반복될 것이다. 다음 그림과 같이 진행될 것이다. 

<img width="1025" alt="복잡" src="https://user-images.githubusercontent.com/127461144/235988333-94f57624-6bc8-4f27-8266-b77df242d0d4.png">

이 문제를 해결하기 위해서 처음부터 $X = \{X_0, X_4, X_2, X_6, X_1, X_5, X_3, X_7\}$과 같이 설정한다면 이웃한 수열끼리만 연산을 하면 된다. 이웃한 수열끼리 연산을 한다면 짝수와 홀수를 합치는 과정에서 추가적인 메모리 할당없이 현재 메모리를 재활용하면서 계산할 수 있다.

FFT를 구현하기 전에 $X$를 정렬하자.

```
vector<complex<double>> b(a);  
for (int i = 0; i < n; i++) {  
    int len = n, startIdx = 0, idx = i;  
    for(int len = n; len > 1; len >>= 1) {  
        if (idx & 1) startIdx += len >> 1;  
        idx >>= 1;  
    }  
    a[startIdx + idx] = b[i];  
}  
```
여기에서 더 알아야 할 것은 $X_k$의 위치이다.

$X_000 -> 000$ (0번째)  
$X_001 -> 100$ (4번째)   
$X_010 -> 010$ (2번째)  
$X_011 -> 110$ (6번째)  
$X_100 -> 001$ (1번째)  
$X_101 -> 101$ (5번째)  
$X_110 -> 011$ (3번째)  
$X_111 -> 111$ (7번째)  

k의 위치는 k를 2진수로 변환했을 때 비트를 뒤집은 곳으로 이동한다.
따라서 다음과 같이 구현할 수 있다.
```
void fft(vector<complex<double>> &a, bool inverse) {  
    int n = a.size();  

    for(int i = 0; i < n; i++) {  
        int rev = 0;  
        for(int j = 1, target = i; j < n; j <<= 1, target >>= 1) {  
            rev = (rev << 1) + (target & 1);  
        }  
        if(i < rev) {  
            swap(a[i], a[rev]);  
        }  
    }  

    for(int len = 2; len <= n; len <<= 1) {  
        double x = 2 * M_PI / len * (inverse ? -1 : 1);  
        complex<double> diff(cos(x), sin(x));  
        for(int i = 0; i < n; i += len) {  
            complex<double> e(1);  
            for(int j = 0; j < len/2; j++) {  
                int cur = i + j;  
                complex<double> even = a[cur];  
                complex<double> oddE = a[cur + len/2] * e;  
                a[cur] = even + oddE;  
                a[cur + len/2] = even - oddE;  
                e *= diff;  
            }  
        }  
    }  
    if(inverse) {  
        for(int i = 0; i < n; i++) {  
            a[i] /= n;  
        }  
    }  
}  
```

# 활용 분야
FFT알고리즘은 전파 천문학 등 다양한 과학 분야에서 디지털 신호 처리 및 이미지 분석 등을 할 때 사용되며, 함수의 근삿값을 계산하는 알고리즘이다. FFT알고리즘은 주파수를 함수로 대체 시각화하여 복잡한 전자파 성질을 이해를 돕는다.

실생활에서는 고속 푸리에 변환(FFT)이나 역고속푸리에변환(IFFT)은 음악을 스트리밍하거나, 휴대폰 신호를 생성할 때, 인터넷을 검색하거나, 셀카를 찍을 때 항상 사용될 만큼 많이 사용된다.

![FFT 결과](https://user-images.githubusercontent.com/127461144/235936838-ba12864d-a617-4a4c-b124-530663683b01.png)






