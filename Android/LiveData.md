# π§΄ Live Data

## κ°μ

### LiveDataλ?
* **LiveData**λ κ΄μ°° κ°λ₯ν λ°μ΄ν° νλ ν΄λμ€μλλ€.
* μ‘ν°λΉν°, νλκ·Έλ¨ΌνΈ, μλΉμ€ λ±μ λ€λ₯Έ μ± κ΅¬μ±μμμ μλͺ μ£ΌκΈ°λ₯Ό μΈμν©λλ€. κ·Έλ κΈ° λλ¬Έμ νλ μλͺ μ£ΌκΈ° μνμ μλ μ± κ΅¬μ±μμ κ΄μ°°μλ§ μλ°μ΄νΈν©λλ€.

### LiveData μ¬μ©μ μ΄μ 
* **UIμ λ°μ΄ν° μνμ μΌμΉ λ³΄μ₯**  
  : κ΄μ°°μ ν¨ν΄μ λ°λ₯΄λ―λ‘ λ°μ΄ν°κ° λ³κ²½λ  λ `Observer`κ°μ²΄μ μλ¦¬λ©΄μ UIμ λ°μ΄ν° μνλ₯Ό λκΈ°νμν΅λλ€.  
    
* **λ©λͺ¨λ¦¬ λμ μμ**  
  : κ΄μ°°μλ `Lifecycle`κ°μ²΄μ κ²°ν©λμ΄ μμΌλ©° μ°κ²°λ μλͺ μ£ΌκΈ°κ° λλλ©΄ μλμΌλ‘ μ­μ λ©λλ€.
    
* **μ€μ§λ νλμΌλ‘ μΈν λΉμ μ μ’λ£ μμ**  
  : μ‘ν°λΉν°κ° λ°± μ€νμ μμ λλ₯Ό λΉλ‘―νμ¬ κ΄μ°°μμ μλͺ μ£ΌκΈ°κ° λΉνμ± μνμ μμΌλ©΄ κ΄μ°°μλ LiveData μ΄λ²€νΈλ₯Ό μμ  λ°μ§ μμ΅λλ€. 
    
* **μλͺ μ£ΌκΈ°λ₯Ό λ μ΄μ μλμΌλ‘ μ²λ¦¬νμ§ μμ**  
  : κ΄μ°°νλ λμ κ΄λ ¨ μλͺ μ£ΌκΈ° μν λ³κ²½μ μΈμνλ―λ‘ λͺ¨λ  κ²μ μλμΌλ‘ κ΄λ¦¬νλ€.
    
* **μ΅μ  λ°μ΄ν° μ μ§**  
  : μλͺ μ£ΌκΈ°κ° λΉνμ± μνμμ νμ±ν λ  λλ, μ± κ΅¬μ± λ³κ²½μ΄ μ΄λ£¨μ΄μ‘μ λ, μ΅μ  λ°μ΄ν°λ₯Ό μμ ν©λλ€.   
  **ex) νλ©΄ νμ , λ°±κ·ΈλΌμ΄λ -> ν¬κ·ΈλΌμ΄λ μ ν**
    
* **λ¦¬μμ€ κ³΅μ **  
  : `LiveData` κ°μ²΄κ° μμ€ν μλΉμ€μ ν λ² μ°κ²°λλ©΄ λ¦¬μμ€κ° νμν λͺ¨λ  κ΄μ°°μκ° `LiveData`κ°μ²΄λ₯Ό λ³Ό μ μμ΅λλ€.


 ## νλ‘μ νΈ μμ 
 
 ### μ’μ ν­λͺ© μΆκ°
 μ± μμ€μ `build.gradle`μμ λ€μ μ’μ ν­λͺ©μ μΆκ°ν©λλ€.
```kotlin
implementation 'androidx.lifecycle:lifecycle-livedata-ktx:2.4.1'
```

### LiveData κ°μ²΄ λ§λ€κΈ°
  νΉμ  μ νμ λ°μ΄ν°λ₯Ό λ³΄μ ν  `LiveData`μ μΈμ€ν΄μ€λ₯Ό μμ±ν©λλ€. μΌλ°μ μΌλ‘ `ViewModel`ν΄λμ€ λ΄μμ μ΄λ£¨μ΄μ§λλ€.  
 
 `ViewModel` ν΄λμ€ λ΄μμ μ΄λ£¨μ΄μ§λ μ΄μ λ LiveData μΈμ€ν΄μ€λ₯Ό νΉμ  μ‘ν°λΉν°λ νλκ·Έλ¨ΌνΈ μΈμ€ν΄μ€μμ λΆλ¦¬νκ³  **κ΅¬μ± λ³κ²½(νλ©΄ νμ  λ±)μλ LiveData κ°μ²΄κ° μ μ§λλλ‘ νκΈ° μν¨**μλλ€.
 ```kotlin
class MainViewModel: ViewModel() {
  var result: MotableLiveData<String> = MutableLiveData()
  
  // LiveData κ°±μ 
  fun setName(value: String) {
    result.value = value
  }
  fun getName(): MutableLivaData<String> = result // LivaData λ°ν
}
```

### νλκ·Έλ¨ΌνΈ νΉμ μ‘ν°λΉν°μ κ΄μ°°μ κ°μ²΄λ₯Ό λκ³  LiveDataλ₯Ό κ΄μ°°
λλΆλΆμ κ²½μ° μ± κ΅¬μ±μμμ `onCreate()` λ©μλκ° `LiveData`κ°μ²΄ κ΄μ°°μ μμνκΈ° μ ν©ν μ₯μμ΄λ©° μ΄μ λ λ€μκ³Ό κ°μ΅λλ€.
* μμ€νμ΄ μ‘ν°λΉν°λ νλκ·Έλ¨ΌνΈμ onResume() λ©μλμμ μ€λ³΅ νΈμΆμ νμ§ μλλ‘ νκΈ° μν¨μλλ€.
* μ‘ν°λΉν°λ νλκ·Έλ¨ΌνΈμ νμ± μνκ° λλ μ¦μ νμν  μ μλ λ°μ΄ν°κ° ν¬ν¨λλλ‘ νκΈ° μν¨μλλ€.

λ€μ μ½λλ `LiveData` κ°μ²΄ κ΄μ°°μ μμνλ λ°©λ²μ λ³΄μ¬μ€λλ€.

```kotlin
class MainActivity : AppCompatActivity() {
    private lateinit var viewModel: MainViewModel
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        // μ½λ μλ΅..
        
        // onChanged() λ©μλλ₯Ό μ μνλ Observer κ°μ²΄λ₯Ό λ§λ­λλ€. μ΄ λ©μλλ LiveDataκ°μ²΄κ° λ³΄μ ν λ°μ΄ν° λ³κ²½μ λ°μνλ μμμ μ μ΄ν©λλ€. 
        val resultObserver = Observer<String> {
            result -> binding.tvResult.text = result.toString()
        }

        // observe()λ₯Ό μ¬μ©νμ¬ LiveDataκ°μ²΄μ LifecycleOwnerλ₯Ό μ¬μ©νμ¬ Observer κ°μ²΄λ₯Ό μ°κ²°ν©λλ€.
        // observe()λ₯Ό νΈμΆνλ©΄ onChanged() μ½λ°± ν¨μκ° μ¦μ νΈμΆλμ΄ getName()μ μ μ₯λ μ΅μ  κ°μ μ κ³΅νλ€.
        viewModel.getName().observe(this, resultObserver)
        binding.button.setOnClickListener {
            if (binding.etName.text.toString().isNotEmpty()) {
                // LiveData κ°μ²΄ μλ°μ΄νΈ
                viewModel.setName(binding.etName.text.toString())
            } else {
                binding.tvResult.text = "No value"
            }
        }
    }
}
```

## LiveData νμ₯
κ΄μ°°μμ μλͺ μ£ΌκΈ°κ° **STARTED** λλ **RESUMED** μνμ΄λ©΄ LiveDataλ κ΄μ°°μλ₯Ό νμ±μνλ‘ κ°μ£Όν©λλ€.
```kotlin
class StockLiveData(symbol: String) : LiveData<BigDecimal>() {
    private val stockManager = StockManager(symbol)

    private val listener = { price: BigDecimal ->
        value = price
    }
    // onActive(): LivaData κ°μ²΄μ νμ± μνμ κ΄μ°°μκ° μμ λ νΈμΆλ©λλ€.
    override fun onActive() {
        stockManager.requestPriceUpdates(listener)
    }
    // onInactive(): LiveData κ°μ²΄μ νμ± μνμ κ΄μ°°μκ° μμ λ νΈμΆλ©λλ€.
    override fun onInactive() {
        stockManager.removeUpdates(listener)
    }
    // setValue(T)λ©μλλ μλλ°, μ΄λ LiveData μΈμ€ν΄μ€μ κ°μ μλ°μ΄νΈνκ³  λͺ¨λ  νμ± μνμ κ΄μ°°μμκ² λ³κ²½μ¬ν­μ μλ¦½λλ€.
}
```
κ·Έλ¦¬κ³  λ€μκ³Ό κ°μ΄ νλκ·Έλ¨ΌνΈμμ ν΄λμ€λ₯Ό μ¬μ©ν  μ μμ΅λλ€.
```kotlin
class MyFragment : Fragment() {

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        StockLiveData.get(symbol).observe(viewLifecycleOwner, Observer<BigDecimal> { price: BigDecimal? ->
            // Update the UI.
        })

    }
```

## LiveData λ³ν
κ΄μ°°μμκ² `LiveData`κ°μ²΄λ₯Ό μ λ¬νκΈ° μ μ κ°μ²΄μ μ μ₯λ κ°μ λ³κ²½νκ³  μΆκ±°λ λ€λ₯Έ κ°μ²΄μ κ°μ λ°λΌ λ€λ₯Έ `LiveData`μΈμ€ν΄μ€λ₯Ό λ°νν΄μΌ νλ κ²½μ°κ° μμ΅λλ€. `Lifecycle`ν¨ν€μ§λ μ΄λ¬ν μλλ¦¬μ€λ₯Ό μ§μνλ λμ°λ―Έ λ©μλκ° ν¬ν¨λ `Transformations`ν΄λμ€λ₯Ό μ κ³΅ν©λλ€.
```kotlin
val userLiveData: LiveData<User> = UserLiveData()
// userLiveDataμ μλ κ°μ μΌλΆλ₯Ό String νμμ userName LiveDataλ‘ λ³ννμ¬ μ μ₯ν  μ μμ΅λλ€.
val userName: LiveData<String> = Transformations.map(userLiveData) {
    user -> "${user.name} ${user.lastName}"
}
```
`Transformations.switchMap()`
  map()κ³Ό λ§μ°¬κ°μ§λ‘ LiveDataκ°μ²΄μ μ μ₯λ κ°μ ν¨μλ₯Ό μ μ©νκ³  κ²°κ³Όλ₯Ό λν ν΄μ νμ¬ λ€μ΄μ€νΈλ¦ΌμΌλ‘ μ λ¬ν©λλ€. switchMap()μ μ λ¬λ ν¨μλ λ€μ μμ κ°μ΄`LiveData`κ°μ²΄λ₯Ό λ°νν΄μΌ ν©λλ€.
```kotlin
private fun getUser(id: String): LiveData<User> {
  ...
}
val userId: LiveData<String> = ...
// userId LiveDataμ μ μ₯λμ΄μλ κ°μ getUser(id) ν¨μλ₯Ό μ μ©μμΌ LiveData<User> κ°μ²΄λ₯Ό λ°νν©λλ€.
val user = Transformations.switchMap(userId) { id -> getUser(id) }
```
## References
[LiveData κ°μ | Android Developers](https://developer.android.com/topic/libraries/architecture/livedata?hl=ko#kotlin)
