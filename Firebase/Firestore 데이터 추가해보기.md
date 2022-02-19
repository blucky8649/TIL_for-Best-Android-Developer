안녕하세요! 저번에는 Firebase에 안드로이드 스튜디오를 연동하는 것 까지 같이 해보았는데요.

이번 포스팅에서는 직접 데이터를 추가해보는 시간을 가져보도록 합시다!

## Let's get Started !

### step 1. 의존성 추가
앱 수준의 gradle을 열고 뷰 바인딩, 코루틴 작성을 위한 의존성을 추가하고 `sync now`를 눌러줍시다.
```kotlin
android {
	/* 중략 */
    buildFeatures {
    	viewBinding true
    }
}
dependencies {
	def coroutines_version = "1.5.0"
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-core:$coroutines_version")
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-android:$coroutines_version")
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-play-services:1.1.1")
}
```

### step 2. 레이아웃 디자인
레이아웃은 성, 이름, 나이를 입력할 수 있도록 EditText를 사용하여 구성하였고, 데이터 베이스에 추가할 수 있는 버튼을 구성하였습니다.

```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:padding="16dp"
        tools:context=".MainActivity">

    <EditText
            android:id="@+id/etFirstName"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:ems="10"
            android:hint="First Name"
            android:inputType="textPersonName"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintHorizontal_bias="0.5"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toTopOf="parent" />

    <EditText
            android:id="@+id/etLastName"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="16dp"
            android:hint="Last Name"
            android:ems="10"
            android:inputType="textPersonName"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintHorizontal_bias="0.5"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toBottomOf="@+id/etFirstName" />

    <EditText
            android:id="@+id/etAge"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="16dp"
            android:hint="Age"
            android:ems="10"
            android:inputType="textPersonName"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintHorizontal_bias="0.5"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toBottomOf="@+id/etLastName" />

    <Button
            android:id="@+id/btnUploadData"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_marginTop="8dp"
            android:text="Save to database"
            app:layout_constraintEnd_toEndOf="@+id/etAge"
            app:layout_constraintHorizontal_bias="0.5"
            app:layout_constraintStart_toStartOf="@+id/etAge"
            app:layout_constraintTop_toBottomOf="@+id/etAge" />
            
</androidx.constraintlayout.widget.ConstraintLayout>
```

### step 3. MainActivity 코드 작성

```kotlin
package com.ebookfrenzy.fstest

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.widget.Toast
import com.ebookfrenzy.fstest.databinding.ActivityMainBinding
import com.google.firebase.firestore.ktx.firestore
import com.google.firebase.ktx.Firebase
import kotlinx.coroutines.CoroutineScope
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.launch
import kotlinx.coroutines.tasks.await
import kotlinx.coroutines.withContext
import java.lang.Exception

class MainActivity : AppCompatActivity() {
    private lateinit var binding: ActivityMainBinding

    // collection이 "person"인 경로로 콜렉션 레퍼런스를 지정한다.
    private val personCollectionRef = Firebase.firestore.collection("persons")
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)

        binding.btnUploadData.setOnClickListener {
            val firstName = binding.etFirstName.text.toString()
            val lastName = binding.etLastName.text.toString()
            val age = binding.etAge.text.toString().toInt()
            val person = Person(firstName, lastName, age)

            savePerson(person)
        }
    }

    private fun savePerson(person: Person) = CoroutineScope(Dispatchers.IO).launch {
        //withContext는 다른 스레드로 포커스를 전환하는 메서드입니다!
        try {
            personCollectionRef.add(person).await() // await는 데이터가 성공적으로 업로드가 될 때 까지 기다려주는 메서드입니다.
            withContext(Dispatchers.Main) {
                Toast.makeText(this@MainActivity, "데이터 업로드 성공!", Toast.LENGTH_LONG).show()
            }
        } catch (e: Exception) {
            withContext(Dispatchers.Main) {
                Toast.makeText(this@MainActivity, e.message, Toast.LENGTH_LONG).show()
            }
        }
    }
}
```

### 결과
결과를 실행하여 정보를 입력하고 `SAVE TO DATABASE`를 누르고 FIREBASE 콘솔에 들어가보면 데이터가 기록된 것을 볼 수 있습니다.
(페이지 새로 고침을 해야 보입니다.)
#### ![결과1](https://images.velog.io/images/blucky8649/post/ec135cb0-76c4-4d00-9309-6a08c6c88ff1/image.png)
<앱 화면>

#### ![](https://images.velog.io/images/blucky8649/post/b6f79614-12e1-47ef-99fe-551b8ed0c6ad/image.png)
<Firebbase 콘솔 페이지 화면>


## 출처
[Uploading Data - Firebase Firestore by Philipp Lackner](https://www.youtube.com/watch?v=SEl--hArDGE)
