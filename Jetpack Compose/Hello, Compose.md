![](https://media.vlpt.us/images/blucky8649/post/01be74b3-d19b-484e-be4a-699ec4fdc151/%EC%A0%9C%EB%AA%A9%EC%9D%84-%EC%9E%85%EB%A0%A5%ED%95%B4%EC%A3%BC%EC%84%B8%EC%9A%94_-001.png)

## What is Jetpack Compose?
**Jetpack Compose**는 네이티브 UI를 빌드하기 위한 Android의 최신 도구 키트입니다. 기존 XML을 사용하여 UI를 제작하던 방식을 버리고, Kotlin API를 이용하여 더 빠르고, 직관적으로 UI를 구현할 수 있습니다.

항상 xml를 이용하여 view Component를 구성하고, view 바인딩을 통해 코드로 view에 접근했다면, compose는 xml 파일 없이 MainActivity.kt에 ui를 코틀린으로 직접 구현 하는 방식입니다.

따라서 이는 코틀린 네이티브로 빌드가 되기 때문에 더 빌드타임이 더 빨라지겠죠? 또한, xml로 구현하지 못하는 생동감 있는 ui도 구현할 수 있다고 하니, 안쓸 이유는 없겠네요.

## Let's get started!

### 1. Android Studio 최신버전 설치 
Compose UI를 안정적으로 사용하기 위해서는 최신 버전(Bulblebee)의 안드로이드 스튜디오를 설치해주시는 것이 좋습니다.

### 2. 프로젝트 생성
File - New - New Project를 클릭하여 프로젝트 템플릿 선택 창으로 간 다음, `Empty Compose Activity`를 선택하여 프로젝트를 생성합니다.
<div><img src="https://images.velog.io/images/blucky8649/post/1d16dfc3-e88e-4d60-a5df-6f6baea665a9/image.png"></div>


### 3. MainActivity.kt 코드 한 눈에 보기
프로젝트를 생성한다면 제일 먼저 보이는 화면입니다.
`setContent`는 ui를 그릴 도화지, 그 안에 있는 요소는 도화지에 담을 콘텐츠라 보시면 될것 같습니다.

`modifier`는 컨텐츠를 담은 상위뷰의 성질(사이즈, 색 등..)의 정의를 담당합니다.

하단의 함수를 보시면 위에 `@Composable`이라는 애노테이션이 붙어있는 것을 보실 수 있습니다. compose ui에 대한 함수를 작성하기 위해서는 위에 해당 애노테이션을 붙여야 합니다.

`@Preview`애노테이션은 Design탭에서 바로 결과물을 보여주기 위한 애노테이션입니다.

```kotlin
package com.example.composepractice

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.material.MaterialTheme
import androidx.compose.material.Surface
import androidx.compose.material.Text
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier
import androidx.compose.ui.tooling.preview.Preview
import com.example.composepractice.ui.theme.ComposePracticeTheme

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            ComposePracticeTheme {
                // A surface container using the 'background' color from the theme
                Surface(
                    modifier = Modifier.fillMaxSize(), // = match parent
                    color = MaterialTheme.colors.background
                ) {
                    Greeting("Android") // Hello Android 라는 텍스트를 넣는다.
                }
            }
        }
    }
}

@Composable
fun Greeting(name: String) {
    Text(text = "Hello $name!")
}

@Preview(showBackground = true)
@Composable
fun DefaultPreview() {
    ComposePracticeTheme {
        Greeting("Android")
    }
}
```

다음 포스팅에는 Row, Column을 이용하여 텍스트를 세로, 혹은 가로로 배치해보면서 modifier를 수정해보는 시간을 가지겠습니다.

참고로 공부한 내용은 **Branch**로 나눠서 정리하고 있습니다.
