---
layout: posts
title: "[Java] 한글 형식의 숫자를 int형으로 변환하기"
comments: true
categories: [Java]
---

```java

import java.util.StringTokenizer;

public class main {

    public static int read(char[] charTextArr, char[] charNumArr, char[] charUnitArr, int[] intArr, int[] intUnitArr) {
        int result = 0;

        int intNum = 1;                              //단위의 갯수를 나타낼 변수 ex) 70 => 7
        int intUnit = 1;                           //단위를 나타낼 변수 ex) 70 => 10

        boolean lastUnit = false;
        for(int i = 0; i < charTextArr.length; i++) {      //시리가 만들어준 텍스트의 길이만큼

            for(int j = 0; j < charNumArr.length; j++) {   //현재 텍스트가 숫자인지 확인
                if(charTextArr[i] == charNumArr[j]) {      //숫자인 경우
                    intNum = intNum * intArr[j];         //단위 갯수에 텍스트에 대응하는 int값을 곱함
                }
            }

            for(int j = 0; j < charUnitArr.length; j++) {   //현재 텍스트가 단위인지 확인
                if(charTextArr[i] == charUnitArr[j]) {   //단위일 경우
                    intUnit = intUnit * intUnitArr[j];      //단위에 텍스트에 대응하는 int값을 곱함
                    result = result + intNum * intUnit; //단위 갯수 * 단위를 결과에 저장
                    intUnit = 1;
                    intNum = 1;                        //단위 갯수 초기화
                }
            }
            if(i == charTextArr.length -1) {            //일의 단위일경우
                for(int j = 0; j < charUnitArr.length; j++) {   //현재 텍스트가 단위인지 확인
                    if(charTextArr[i] == charUnitArr[j]) {   //단위일 경우
                        lastUnit = true;
                    }
                }
                if(lastUnit == true) {
                    intNum = 0;
                }
                result = result + intNum;   //단위갯수를 결과에 저장
            }
        }
        return result;
    }

    public static void main(String[] args) {
        String strInput = "만천이백일";

        String strNum = "영일이삼사오육칠팔구"; //숫자 식별을 위한 String
        String strUnit = "천백십";                     //단위 식별을 위한 String

        boolean overMan = false;                     //변환할 텍스트가 만보다 큰지 확인할 변수1
        boolean hasOverMan = false;                     //만보다 큰지 확인할 변수2

        String strOverMan="";                        //만보다 큰 수가 저장될 String
        String strUnderMan="";                        //만보다 작은 수가 저장될 String

        for(int i =0; i < strInput.length(); i++) {         //시리가 만들어준 텍스트가 만보다 큰지 확인
            if(strInput.charAt(i) == '만') {               //클 경우 OverMan = true;
                overMan = true;
            }
        }

        if(strInput.split("만")[0].equals("")) {
            strInput = "일" + strInput; // 앞자리가 만인 경우 일만이 되도록 수정해줌
        }

        StringTokenizer st = new StringTokenizer(strInput, "만");   //만을 기준으로 String을 앞뒤로 자름

        while(st.countTokens() > 0) {                  //토큰이 더 이상 없을 때까지 반복
            if(overMan == true) {                     //만보다 클 경우
                strOverMan = st.nextToken();            //만보다 큰 수를 저장하는 String에 저장
                hasOverMan = true;                     //만보다 큰 적이 있다라는 Flag true
                overMan = false;                     //Flag 반전
            }else {
                strUnderMan = st.nextToken();            //만보다 작은 경우 만보다 작은 수를 저장하는 String에 저장
            }
        }

        int intArr[] = {0,1,2,3,4,5,6,7,8,9,10};         //식별한 숫자에 대응 하는 int
        int intUnitArr[] = {1000,100,10};            //식별한 단위에 대응하는 int

        int intResult = 0;                           //단위의 갯수 * 단위을 저장할 변수 ex) 7 * 70 = 70

        //C이기 때문에 char로 만드는 과정
        char[] charNumArr = strNum.toCharArray();         //숫자 식별을 위한 String을 char로
        char[] charUnitArr = strUnit.toCharArray();         //단위 식별을 위한 String을 char로

        char[] charOverMan = strOverMan.toCharArray();      //만보다 높은 자리의 수가 저장된 String을 char로
        char[] charUnderMan = strUnderMan.toCharArray();   //만보다 낮은 자리의 수가 저장된 String을 char로

        System.out.println("입력된 텍스트 : " + strInput);      //입력 텍스트 확인


        if(hasOverMan == true) {                     //만보다 높은 숫자  경우
            intResult = intResult + read(charOverMan, charNumArr, charUnitArr, intArr, intUnitArr) * 10000;
        }
        intResult = intResult + read(charUnderMan, charNumArr, charUnitArr, intArr, intUnitArr);
        System.out.println("total : " + intResult);
    }
}
```