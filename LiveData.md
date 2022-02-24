# 🧴 Live Data

## 개요

### LiveData란?
* **LiveData**는 관찰 가능한 데이터 홀더 클래스입니다.
* 액티비티, 프래그먼트, 서비스 등의 다른 앱 구성요소의 수명 주기를 인식합니다. 그렇기 때문에 활동 수명 주기 상태에 있는 앱 구성요소 관찰자만 업데이트합니다.

### LiveData 사용의 이점
* **UI와 데이터 상태의 일치 보장**  
  : 관찰자 패턴을 따르므로 데이터가 변경될 때 `Observer`객체에 알리면서 UI와 데이터 상태를 동기화시킵니다.  
    
* **메모리 누수 없음**  
  : 관찰자는 `Lifecycle`객체에 결합되어 있으며 연결된 수명 주기가 끝나면 자동으로 삭제됩니다.
    
* **중지된 활동으로 인한 비정상 종료 없음**  
  : 액티비티가 백 스택에 있을 때를 비롯하여 관찰자의 수명 주기가 비활성 상태에 있으면 관찰자는 LiveData 이벤트를 수신 받지 않습니다. 
    
* **수명 주기를 더 이상 수동으로 처리하지 않음**  
  : 관찰하는 동안 관련 수명 주기 상태 변경을 인식하므로 모든 것을 자동으로 관리한다.
    
* **최신 데이터 유지**  
  : 수명 주기가 비활성 상태에서 활성화 될 때나, 앱 구성 변경이 이루어졌을 때, 최신 데이터를 수신합니다.   
  **ex) 화면 회전, 백그라운드 -> 포그라운드 전환**
    
* **리소스 공유**  
  : 
  
  
 ### Live Data 사용법
 
 #### 종속 항목 추가
 앱 수준의 `build.gradle`에서 다음 종속 항목을 추가합니다.
```kotlin
implementation 'androidx.lifecycle:lifecycle-livedata-ktx:2.4.1'
```

#### LiveData 선언
 관찰하고자 하는 데이터를 넣을 LiveData를 선언하고 get, set 메서드를 작성합니다.
 ```kotlin
class MainViewModel: ViewModel() {
  var result: MotableLiveData<String> = MutableLiveData()
  
  fun setName(value: String) = result.value = value.toString() // LivaData 데이터 갱신
  fun getName(): MutableLivaData<String> = result // LivaData 반환
}
```

#### 프래그먼트 혹은 액티비티에 관찰자 객체를 두고 LiveData를 관찰
대부분의 경우 앱 구성요소의 `onCreate()` 메서드가 `LiveData`객체 관찰을 시작하기 적합한 장소이며 이유는 다음과 같습니다.
* 시스템이 액티비티나 프래그먼트의 onResume() 메서드에서 중복 호출을 하지 않도록 하기 위함입니다.
* 액티비티나 프래그먼트에 활성 상태가 되는 즉시 표시할 수 있는 데이터가 포함되도록 하기 위함입니다.

다음 코드는 `LiveData` 객체 관찰을 시작하는 방법을 보여줍니다.
