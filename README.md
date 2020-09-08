# Arduino
##  1. IRreomte
- 적외선 리모컨과 리시버를 활용하는 과제입니다. `아두이노 중급` 정도 과정에서 활용할 수 있을 것 같습니다. 
- 적외선 리모컨은 일반적으로 아두이노 키트에 함께 들어있는 경우가 많으며,  같이 생겼습니다. 

- 리시버는 몇가지 종류가 있지만, 저는 `IRremote` 모듈을 사용해 보았습니다. 

## 2. Hardware Setting
- 언제나 그러하듯, 저는 회로부터 설계합니다. 회로는 여러 블로그를 다녀보았지만, 이 설계한 것이 가장 안정적인 것 같습니다. 




## 3. Software Coding
#### Library 다운
> 스케치 실행 - 툴 - 라이브러리 관리 - `IRremote` 검색 & 다운

### Sketch Coding
```
#include <IRremote.h> // 다운받은 라이브러리 활용
int input_pin = 2;   //입력핀의 설정

IRrecv irrecv(input_pin);   //IRrecv 객체생성
decode_results signals;   //수신 데이터 저장 구조체 선언
 
void setup()
{
    Serial.begin(9600);     //시리얼 통신
    irrecv.enableIRIn();    //수신시작
    pinMode(13, OUTPUT);  // LED 출력 선언(회로도에 첨부 안함)
}
 
void loop() {
  
//수신되는 내용이 있을 경우만 시리얼모니터에 표시함  
if (irrecv.decode(&signals)) //수신을 받으면
   {   
     if (signals.decode_type == NEC) // NEC 포맷의 IR신호(수신된 적외선 신호를 해석해 16진수 코드로 변환)
       {   
         switch (signals.value) // 수신한 값을 각 case와 비교
         {
          case 0x00FF6897:  //key 0
          Serial.println("key id 0");
          digitalWrite(13, HIGH);
          break;
          
          case 0x00FF30CF:  //key 1
          Serial.println("key id 1");
          digitalWrite(13, LOW);
          break;

          case 0x00FF18E7:  //key2
          Serial.println("key id 2");
          break;
          
         default:
         break;
         }
       }
        irrecv.resume(); //다음값 수신
    }
}
```
### Decode
> 수신된 적외선 신호를 해석해 **16진수 코드**로 변환
- *리모컨 수신 값* 과 *16진수 코드* 
``` 
0x00FF6897 -> 숫자 0
0x00FF30CF -> 숫자 1
0x00FF18E7 -> 숫자 2
0x00FF7A85 -> 숫자 3
0x00FF10EF -> 숫자 4
0x00FF38C7 -> 숫자 5
0x00FF5AA5 -> 숫자 6 
0x00FF42BD -> 숫자 7 
0x00FF4AB5 -> 숫자 8
0x00FF52AD -> 숫자 9
```

[![Watch the video](https://img.youtube.com/vi/qxe3xkCqmlQ/maxresdefault.jpg)](https://youtu.be/qxe3xkCqmlQ)
