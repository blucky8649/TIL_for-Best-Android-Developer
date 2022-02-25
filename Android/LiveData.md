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
  : `LiveData` 객체가 시스템 서비스에 한 번 연결되면 리소스가 필요한 모든 관찰자가 `LiveData`객체를 볼 수 있습니다.


 ## 프로젝트 예제
 
 ### 종속 항목 추가
 앱 수준의 `build.gradle`에서 다음 종속 항목을 추가합니다.
```kotlin
implementation 'androidx.lifecycle:lifecycle-livedata-ktx:2.4.1'
```

### LiveData 객체 만들기
  특정 유형의 데이터를 보유할 `LiveData`의 인스턴스를 생성합니다. 일반적으로 `ViewModel`클래스 내에서 이루어집니다.  
 
 `ViewModel` 클래스 내에서 이루어지는 이유는 LiveData 인스턴스를 특정 액티비티나 프래그먼트 인스턴스에서 분리하고 **구성 변경(화면 회전 등)에도 LiveData 객체가 유지되도록 하기 위함**입니다.
 ```kotlin
class MainViewModel: ViewModel() {
  var result: MotableLiveData<String> = MutableLiveData()
  
  // LiveData 갱신
  fun setName(value: String) {
    result.value = value
  }
  fun getName(): MutableLivaData<String> = result // LivaData 반환
}
```

### 프래그먼트 혹은 액티비티에 관찰자 객체를 두고 LiveData를 관찰
대부분의 경우 앱 구성요소의 `onCreate()` 메서드가 `LiveData`객체 관찰을 시작하기 적합한 장소이며 이유는 다음과 같습니다.
* 시스템이 액티비티나 프래그먼트의 onResume() 메서드에서 중복 호출을 하지 않도록 하기 위함입니다.
* 액티비티나 프래그먼트에 활성 상태가 되는 즉시 표시할 수 있는 데이터가 포함되도록 하기 위함입니다.

다음 코드는 `LiveData` 객체 관찰을 시작하는 방법을 보여줍니다.

```kotlin
class MainActivity : AppCompatActivity() {
    private lateinit var viewModel: MainViewModel
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        // 코드 생략..
        
        // onChanged() 메서드를 정의하는 Observer 객체를 만듭니다. 이 메서드는 LiveData객체가 보유한 데이터 변경시 발생하는 작업을 제어합니다. 
        val resultObserver = Observer<String> {
            result -> binding.tvResult.text = result.toString()
        }

        // observe()를 사용하여 LiveData객체에 LifecycleOwner를 사용하여 Observer 객체를 연결합니다.
        // observe()를 호출하면 onChanged() 콜백 함수가 즉시 호출되어 getName()에 저장된 최신 값을 제공한다.
        viewModel.getName().observe(this, resultObserver)
        binding.button.setOnClickListener {
            if (binding.etName.text.toString().isNotEmpty()) {
                // LiveData 객체 업데이트
                viewModel.setName(binding.etName.text.toString())
            } else {
                binding.tvResult.text = "No value"
            }
        }
    }
}
```

## LiveData 확장
관찰자의 수명 주기가 **STARTED** 또는 **RESUMED** 상태이면 LiveData는 관찰자를 활성상태로 간주합니다.
```kotlin
class StockLiveData(symbol: String) : LiveData<BigDecimal>() {
    private val stockManager = StockManager(symbol)

    private val listener = { price: BigDecimal ->
        value = price
    }
    // onActive(): LivaData 객체에 활성 상태의 관찰자가 있을 때 호출됩니다.
    override fun onActive() {
        stockManager.requestPriceUpdates(listener)
    }
    // onInactive(): LiveData 객체에 활성 상태의 관찰자가 없을 때 호출됩니다.
    override fun onInactive() {
        stockManager.removeUpdates(listener)
    }
    // setValue(T)메서드도 있는데, 이는 LiveData 인스턴스의 값을 업데이트하고 모든 활성 상태의 관찰자에게 변경사항을 알립니다.
}
```
그리고 다음과 같이 프래그먼트에서 클래스를 사용할 수 있습니다.
```kotlin
class MyFragment : Fragment() {

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        StockLiveData.get(symbol).observe(viewLifecycleOwner, Observer<BigDecimal> { price: BigDecimal? ->
            // Update the UI.
        })

    }
```

## LiveData 변환
관찰자에게 `LiveData`객체를 전달하기 전에 객체에 저장된 값을 변경하고 싶거나 다른 객체에 값에 따라 다른 `LiveData`인스턴스를 반환해야 하는 경우가 있습니다. `Lifecycle`패키지는 이러한 시나리오를 지원하는 도우미 메서드가 포함된 `Transformations`클래스를 제공합니다.
```kotlin
val userLiveData: LiveData<User> = UserLiveData()
// userLiveData에 있는 값의 일부를 String 타입의 userName LiveData로 변환하여 저장할 수 있습니다.
val userName: LiveData<String> = Transformations.map(userLiveData) {
    user -> "${user.name} ${user.lastName}"
}
```
`Transformations.switchMap()`
  map()과 마찬가지로 LiveData객체에 저장된 값에 함수를 적용하고 결과를 래핑 해제하여 다운스트림으로 전달합니다. switchMap()에 전달된 함수는 다음 예와 같이`LiveData`객체를 반환해야 합니다.
```kotlin
private fun getUser(id: String): LiveData<User> {
  ...
}
val userId: LiveData<String> = ...
// userId LiveData에 저장되어있는 값에 getUser(id) 함수를 적용시켜 LiveData<User> 객체를 반환합니다.
val user = Transformations.switchMap(userId) { id -> getUser(id) }
```
## References
[LiveData 개요 | Android Developers](https://developer.android.com/topic/libraries/architecture/livedata?hl=ko#kotlin)
