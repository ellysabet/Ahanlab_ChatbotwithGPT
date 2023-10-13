# 1차 과제
***
## 1. github 공개 레포지토리를 만들고 URL을 공개하십시오.
URL : https://github.com/ellysabet/Ahanlab_ChatbotwithGPT

***
## 2. 개발자용 프롬프트 3가지 이상을 작성하시오. ( 그 중 한가지는 UML 과제 )
- 질문 : 걸음 수를 확인할 수 있는 앱을  swift를 이용해 ios 버전으로 만들고 싶어. 해당 코드를 작성해줘
  <details>
  <summary>답변</summary>
    
  걸음 수를 확인하는 앱을 만들려면, iOS의 "Core Motion" 프레임워크를 사용해야 합니다. 이를 활용하여 기본적인 걸음 수 확인 앱을 만드는 방법을 아래에 설명합니다.

  1. 프로젝트 설정: 
     * Xcode에서 새로운 Single View App 프로젝트를 생성합니다.
     * "Info.plist" 파일에 "Privacy - Motion Usage Description" 항목을 추가하고, 사용자에게 왜 걸음 수 정보에 접근해야 하는지 설명하는 메시지를 입력합니다.

  2. Core Motion 프레임워크 추가: 
     * 프로젝트 내의 해당 타겟으로 이동하여 "Build Phases > Link Binary With Libraries" 섹션에서 "CoreMotion.framework"을 추가합니다.

  3. ViewController 작성:
     ```dart
     import UIKit
     import CoreMotion

     class ViewController: UIViewController {

     let pedometer = CMPedometer()
     @IBOutlet weak var stepsLabel: UILabel!

     override func viewDidLoad() {
        super.viewDidLoad()

        if CMPedometer.isStepCountingAvailable() {
            startUpdating()
        } else {
            stepsLabel.text = "걸음 수 계산을 지원하지 않는 장치입니다."
        }
     }

     func startUpdating() {
        pedometer.startUpdates(from: Date()) { [weak self] pedometerData, error in
            DispatchQueue.main.async {
                if let error = error {
                    self?.stepsLabel.text = "에러: \(error.localizedDescription)"
                    return
                }
                if let steps = pedometerData?.numberOfSteps.intValue {
                    self?.stepsLabel.text = "걸음 수: \(steps)"
                }
            }
        }
     }
     }

     ```
    
  4.  Interface Builder (IB):
      * "Main.storyboard" 파일을 열어서 "UILabel"을 추가하고 "stepsLabel"에 연결하세요.

  5.  실행 :
      * 앱을 실행하면 실시간으로 걸음 수가 표시됩니다.

   이 코드는 간단한 걸음 수 앱의 기본 구조를 제공합니다. 추가적인 기능을 추가하거나 UI를 개선하려면 추가적인 구현이 필요합니다.
</details>
<br>
<br>

- 질문 : 네가 제시한 내용을 mermaid flow chart를 이용해 UML로 만들어줘 / 제시한 플로우차트를 시퀀스 다이어그램과 클래스 다이어그램으로  그려줘
  <details>
  <summary>답변</summary>
    
  제시한 내용을 기반으로 시퀀스 다이어그램과 클래스 다이어그램을 mermaid를 사용하여 작성하겠습니다.

  1. 시퀀스 다이어그램: 
     * 시퀀스 다이어그램은 객체 간의 상호 작용을 시간 순서대로 나타냅니다.
     ```dart
     sequenceDiagram
     participant User as 사용자
     participant Xcode as Xcode
     participant Project as 프로젝트
     participant ViewController as ViewController
     participant Pedometer as CMPedometer

     User->>Xcode: Single View App 프로젝트 생성
     activate Xcode
     Xcode->>Project: 프로젝트 생성
     deactivate Xcode
     activate Project
     Project->>Project: Info.plist에 설명 추가
     Project->>Project: CoreMotion.framework 추가
     deactivate Project
     User->>ViewController: 앱 실행
     activate ViewController
     ViewController->>Pedometer: 걸음 수 요청
     Pedometer-->>ViewController: 걸음 수 응답
     deactivate ViewController
     ```  
       
  
     ![Alt text](image-2.png)


  2. 클래스 다이어그램: 
     * 클래스 다이어그램은 시스템의 클래스와 그들 간의 관계를 나타냅니다.
     ```dart
     classDiagram
     class Project {
        +createNewProject()
        +addDescriptionToPlist()
        +linkCoreMotionFramework()
     }

     class ViewController {
        -pedometer: CMPedometer
        +viewDidLoad()
        +startUpdating()
     }

     class CMPedometer {
        +isStepCountingAvailable() : bool
        +startUpdates(from: Date, handler: (Data?, Error?))
     }

     Project "1" --> "1" ViewController: contains
     ViewController "1" --> "1" CMPedometer: uses
     ```  
     ![Alt text](image-3.png)

    이는 기본적인 시퀀스와 클래스 다이어그램의 표현입니다. 실제 시스템에서는 더 많은 클래스와 메소드, 속성 등이 있을 수 있습니다.
</details>

<br>
<br>

- 질문 : 위의 ios 걸음 측정 앱을 안드로이드로 개발할 수 있도록 코드를 수정해줘
  <details>
  <summary>답변</summary>
    
  iOS의 걸음 측정 앱을 안드로이드 버전으로 변환하려면, Android의 SensorManager API를 사용해야 합니다. 아래는 Android에서 걸음 수를 확인하는 기본 앱을 만드는 방법에 대한 설명과 코드입니다.

  1. 프로젝트 설정: 
     * Android Studio에서 새로운 프로젝트를 생성합니다.
     * "AndroidManifest.xml" 파일에 다음 권한을 추가합니다 
     ```dart
     xml
     <uses-permission android:name="android.permission.ACTIVITY_RECOGNITION" />

     ```



  2. MainActivity 작성: 
     ```dart
     java
     package com.example.stepcounter;

     import androidx.annotation.NonNull;
     import androidx.appcompat.app.AppCompatActivity;
     import android.content.Context;
     import android.hardware.Sensor;
     import android.hardware.SensorEvent;
     import android.hardware.SensorEventListener;
     import android.hardware.SensorManager;
     import android.os.Bundle;
     import android.widget.TextView;

     public class MainActivity extends AppCompatActivity implements SensorEventListener {

     private SensorManager sensorManager;
     private TextView stepsLabel;
     private Sensor stepCounter;
     private int stepCount = 0;

     @Override
     protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        stepsLabel = findViewById(R.id.stepsLabel);

        sensorManager = (SensorManager) getSystemService(Context.SENSOR_SERVICE);
        if (sensorManager != null) {
            stepCounter = sensorManager.getDefaultSensor(Sensor.TYPE_STEP_COUNTER);
        }
     }

     @Override
     protected void onResume() {
        super.onResume();
        if (stepCounter != null) {
            sensorManager.registerListener(this, stepCounter, SensorManager.SENSOR_DELAY_NORMAL);
        }
     }

     @Override
     protected void onPause() {
        super.onPause();
        sensorManager.unregisterListener(this);
     }

     @Override
     public void onSensorChanged(SensorEvent event) {
        if (event.sensor.getType() == Sensor.TYPE_STEP_COUNTER) {
            stepCount = (int) event.values[0];
            stepsLabel.setText(String.format("걸음 수: %s", stepCount));
        }
     }

     @Override
     public void onAccuracyChanged(Sensor sensor, int accuracy) {
        // Not used in this example
     }
     }

  3. 레이아웃 작성 (res/layout/activity_main.xml):
     ```dart
     xml
     <?xml version="1.0" encoding="utf-8"?>
     <androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
     xmlns:app="http://schemas.android.com/apk/res-auto"
     xmlns:tools="http://schemas.android.com/tools"
     android:layout_width="match_parent"
     android:layout_height="match_parent"
     tools:context=".MainActivity">

     <TextView
        android:id="@+id/stepsLabel"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="걸음 수: 0"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

     </androidx.constraintlayout.widget.ConstraintLayout>

     ```

  이 코드는 안드로이드에서 걸음 수를 확인하는 간단한 앱의 기본 구조를 제공합니다. 추가 기능이나 디자인 개선을 위해서는 추가적인 구현과 수정이 필요합니다.
</details>

<br>
<br>

***
## 3. ADA 프롬프트 3가지이상을 작성하시오.
- 질문 : 위의 CSV 파일을 분석해서 ptratio와 주택가격의 상관관계를 그래프로 나타내줘  
 !(https://drive.google.com/file/d/1XEHUVGMJJ7V-GhSTPp_ZBLgA3MO9XU_H/view?usp=drive_link)
  <details>
  <summary>답변</summary>
    
  먼저, 제공된 CSV 파일을 읽어보겠습니다.

  제공된 데이터에서 'PTRATIO' (학생/교사 비율)과 'Target' (주택 가격)의 상관관계를 시각화하겠습니다.

  먼저 산점도(scatter plot)를 사용하여 두 변수 간의 관계를 시각화하겠습니다.  
  ![Alt text](image-1.png)
  


  산점도를 통해 'PTRATIO' (학생/교사 비율)와 주택 가격 사이의 관계를 볼 수 있습니다. 전반적으로 PTRATIO가 높을수록 주택 가격이 낮아지는 경향을 보이는 것 같습니다.

  물론, 이것만으로 두 변수 사이의 상관관계를 확정짓기는 어렵습니다. 상관계수를 계산하여 두 변수 사이의 선형 관계의 강도를 확인하겠습니다.

  'PTRATIO'와 주택 가격 사이의 상관계수는 약 -0.508입니다. 이는 두 변수 사이에 중간 정도의 음의 선형 관계가 있음을 나타냅니다. 즉, 'PTRATIO'가 높아질수록 주택 가격이 일반적으로 낮아지는 경향이 있음을 의미합니다.

  그러나 상관계수만으로 인과 관계를 결론 내릴 수는 없습니다. 상관계수는 단순히 두 변수 사이의 관계의 강도와 방향만을 나타냅니다.


</details>
<br>
<br>

- 질문 : 베스킨라빈스 31 게임의 필승전략을 알려줘 


  <details>
  <summary>답변</summary>
    
  베스킨라빈스 31 게임은 두 명 이상의 참가자가 번갈아가며 1, 2, 3 중 하나의 숫자를 부르면서 합계를 누적하는 게임입니다. 이 게임의 목표는 숫자 31을 부르는 것을 피하는 것입니다. 31을 부르게 되면 그 선수는 패배합니다.

  게임의 필승 전략은 다음과 같습니다:

   1. 게임의 시작: 만약 당신이 게임을 시작한다면, 첫 번째로 '1'을 부르십시오.
   2. 4의 배수를 부르기: 게임에서 이길 확률을 높이기 위해 항상 합계가 4의 배수가 되도록 숫자를 부르는 것이 좋습니다. 즉, 4, 8, 12, 16, 20, 24, 28을 목표로 합니다.
   3. 상대방의 플레이를 예측: 상대방이 이 전략을 알고 있다면, 그들의 움직임을 예측하고 반대 전략을 사용하여 게임을 제어하십시오.


  핵심은 항상 4의 배수를 만들도록 노력하는 것입니다. 이렇게 하면 마지막 턴에서 당신은 30을 부르기 전의 마지막 숫자를 부를 수 있고, 상대방은 31을 부를 수밖에 없게 됩니다.

  예를 들어, 만약 당신이 1을 부르고 상대방이 2와 3을 연속으로 부른다면, 당신은 다음 번에 4를 부를 수 있습니다. 이후 상대방이 어떤 숫자를 부르든 간에 항상 4의 배수를 유지하려고 노력하면 됩니다.

  물론, 이 전략은 상대방도 같은 전략을 알고 있다면 적용하기 어렵습니다. 따라서 게임의 상황과 상대방의 전략에 따라 유동적으로 플레이하면서 4의 배수 전략을 적용하는 것이 중요합니다.

</details>

<br>
<br>

- 질문 : 죄수의 딜레마 문제를 알고 있다면 그 문제를 논리적으로 해결해줘
  <details>
  <summary>답변</summary>
    
  죄수의 딜레마는 게임 이론에서 잘 알려진 문제입니다. 이 문제는 다음과 같이 설정됩니다:  

  두 명의 죄수가 각기 다른 방에 갇혀 있습니다. 그들에게는 다음과 같은 선택이 주어집니다:

    - 반성한다 (협력한다): 다른 죄수가 자신을 배신하지 않을 것이라고 믿는다.
    - 배신한다: 다른 죄수가 자신을 반성하도록 유도하려고 한다.


  선택에 따른 결과는 다음과 같습니다:

  두 죄수 모두 반성하면, 각각 2년의 형을 받습니다.  
  한 죄수만 반성하고 다른 죄수가 배신하면, 반성한 죄수는 3년의 형을 받고, 배신한 죄수는 형을 받지 않습니다.  
  두 죄수 모두 배신하면, 각각 1년의 형을 받습니다.  

  논리적 분석:  
  각 죄수의 관점에서 최적의 선택을 결정해보겠습니다.

   - 다른 죄수가 반성할 경우:  
     나도 반성하면 2년의 형을 받음.  
     나는 배신하면 형을 받지 않음.  
        => 이 경우, 배신하는 것이 더 나음.  

   - 다른 죄수가 배신할 경우:  
     나도 반성하면 3년의 형을 받음.
     나는 배신하면 1년의 형을 받음.
        => 이 경우, 배신하는 것이 더 나음.  

  두 가지 시나리오 모두에서, 각 죄수에게는 배신하는 것이 더 나은 결과를 가져다 줍니다. 따라서, 논리적인 선택은 배신입니다.

  그러나 이것은 흥미로운 결과를 가져옵니다. 두 죄수 모두 논리적으로 행동하면, 그들은 각각 1년의 형을 받게 됩니다.   
  반면, 서로 협력한다면 각각 2년의 형만 받게 될 것입니다. 그렇기 때문에 논리적인 개별적 선택이 항상 최적의 집단 결과를 가져오지는 않습니다.

</details>

<br>
<br>

***
## 4. ChatGPT 플러그인 프롬프트 3가지 이상을 작성하시오..
 ### (1) KAYAK
 - 질문 : 3인가족이 12월에 일주일간 제주도에 여행을 할 꺼야. 비행기편과 숙소, 자동차 렌탈 비용을 추천해줘/비행기 출발시간은 오전 8시, 숙소는 에어비앤비로, 자동차는 중형차로  추천해줘

   <details>
   <summary>답변</summary>
    
   3인 가족이 12월에 일주일간 제주도 여행을 위한 추천은 다음과 같습니다:

   1. 비행기편:  
     - 대한항공: ICN(인천) -> CJU(제주)
     - 출발: 12월 1일 10:35, 도착: 12월 1일 20:25
     - 가격: 643,900원
     - 예약 링크(https://www.kayak.co.kr/flights/ICN-CJU/2023-12-01/2023-12-08/3adults/28df2cce5ff485d3ee49b38c627e8d19?a=openai)  

   2. 숙소 (에어비앤비 스타일):  
     - D4A Relaxing house with ocean view in Seopjikoji
     - 가격: 157,697원/박
     - 주요 시설: Kitchen/kitchenette
     - 예약 링크  (https://www.kayak.co.kr/hotels/D4A-Relaxing-house-with-ocean-view-in-Seopjikoji-U-do-c321439-h8199562-details/2023-12-01/2023-12-08/3adults?a=openai) 

   3. 자동차 렌탈 (중형차):  
    - 차종: Intermediate
    - 일일 요금: 12,959원
    - 예약 링크(https://www.kayak.co.kr/cars/Jeju-City-South-Korea-c32610/2023-12-01/2023-12-08/Intermediate/crid-312dcfa2ec2378964614bccb6de2cdcb?a=openai562-details/2023-12-01/2023-12-08/3adults?a=openai)


   이 가격들은 현재 시점의 가격이며, 예약 시점에 따라 변동될 수 있습니다. 원하시는 옵션을 선택하셔서 예약을 진행하시면 됩니다. 좋은 여행 되세요!

</details>
<br>
<br>

 ### (2) There's An AI For It
 - 질문 : 스타벅스 상권분석 딥러닝을 만들고 싶은데 필요한 AI 도구들을 추천해줘

   <details>
   <summary>답변</summary>
    
   스타벅스 상권분석을 위한 딥러닝을 구축하려면 다양한 AI 도구들을 활용할 수 있습니다. 여기 추천하는 AI 도구들은 상권분석과 관련된 데이터 분석 및 딥러닝 작업에 유용할 것입니다:

    1. TaylorAI: TaylorAI는 언어 모델 훈련을 쉽게 할 수 있는 오픈소스 도구입니다.

    2. DataGPTd: DataGPTd는 데이터 시각화를 비전문가도 쉽게 할 수 있게 변환해주는 도구입니다.

    3. Crowd Prisma: Crowd Prisma는 설문조사를 분석하여 빠른 인사이트를 제공합니다.

    4. Hal9: Hal9는 데이터와 대화하며 몇 초 안에 인사이트를 얻을 수 있습니다.

    5. MindsDB: MindsDB는 MindsDB의 개발 플랫폼을 사용하여 앱을 구축하는 데 도움이 됩니다.

   이 외에도 많은 도구들이 있으니, 특정 요구사항이나 기능에 따라 선택하여 사용하시면 됩니다.

</details>
<br>
<br>

### (3) There's An AI For It
 - 질문 : 보라색 바다에서 헤엄치고 있는 초록색 고래의 영상을 만들어줘
   <details>
   <summary>답변</summary>
    
   보라색 바다에서 헤엄치고 있는 초록색 고래의 영상을 만들었습니다!

   🎥 Green Whale in Purple Sea

   설명: A mesmerizing video of a green whale swimming gracefully in a vibrant purple sea.  
   태그: #whale #sea #green #purple #nature  
   중요: 영상을 저장하거나 편집하려면 다음 코드를 사용하세요: 860737. 이 코드는 24시간 동안만 유효합니다.  
    ![Alt text](image.png)

</details>
<br>
<br>