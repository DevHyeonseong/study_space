### Jetpack Compse side-effect



#### 1. LaunchedEffect

- launchedEffect를 사용하여 컴포즈의 라이프사이클에 맞게 코루틴을 사용 할 수 있다
- 오랫동안 뷰가 화면에 보이지않을경우 메모리릭 방지를 위해 코루틴이 자동으로 취소된다

- parameter
  - key1 : key1의 값이 바뀌어서 리컴포지션이 일어나면 코루틴을 취소하고 재시작한다. 관여받고 싶지 않다면 Unit을 넘긴다
  - block : 코루틴 블록

##### code

```kotlin
@Composable
fun BoxLayout() {
    var visible by remember {
        mutableStateOf(true)
    }

    Column(modifier = Modifier.fillMaxSize()) {
        Button(
            onClick = { visible = !visible },
            modifier = Modifier.align(Alignment.CenterHorizontally)
        ) {
            Text(text = "Click!")
        }
        if(visible) {
            Box(
                modifier = Modifier
                    .background(Color.Green)
                    .size(50.dp)
                    .align(Alignment.CenterHorizontally)
            ) {
                LaunchedEffect(key1 = Unit, block = {
                    try{
                        println("Timer Start")
                        delay(10000L)
                        println("Timer End")
                    } catch (e : Exception){
                        println("Timer cancelled")
                    }
                })
            }
        }
    }
}
```

- 버튼을 클릭해서 visible = false로 바뀌면 Box가 보이지 않으므로 실행중이던 코루틴이 취소된다.

```kotlin
@Composable
fun BoxLayout() {
    var count by remember {
        mutableStateOf(1)
    }

    Column(modifier = Modifier.fillMaxSize()) {
        Button(
            onClick = { count++ },
            modifier = Modifier.align(Alignment.CenterHorizontally)
        ) {
            Text(text = "Click!")
        }
        Box(
            modifier = Modifier
                .background(Color.Green)
                .size(50.dp)
                .align(Alignment.CenterHorizontally)
        ) {
            LaunchedEffect(key1 = count, block = {
                try {
                    println("Timer Start")
                    delay(10000L)
                    println("Timer End")
                } catch (e: Exception) {
                    println("Timer cancelled")
                }
            })
        }
    }
}
```

- key1이 count값을 바라보고 있으므로 버튼을 클릭해서 값이 변하면 코루틴이 취소됐다가 재시작된다.



#### 2. rememberCoroutineScope

- LaunchedEffect는 Composable함수이므로 기본적으로 다른 composable 함수에서만 호출 할 수 있다
- LaunchedEffect를 사용하면 coroutine의 lifecycle을 컨트롤 할 수 없다. 코루틴의 시작과 종료는 Composable lifecycle에 기반한다

##### code

```kotlin
@Composable
fun BoxLayout() {
    var count by remember {
        mutableStateOf(1)
    }
    var visible by remember {
        mutableStateOf(true)
    }
    val scope = rememberCoroutineScope()

    Column(modifier = Modifier.fillMaxSize()) {
        Button(onClick = {
            println("Timer started")
            scope.launch {
                try {
                    delay(5000L)
                    println("Timer ended")
                } catch (e: Exception) {
                    println("Timer cancelled")
                }
            }
        }) {
            Text(text = "Start Timer")
        }
    }
}
```

- 컴포지션이 종료될 때 코루틴이 실행중이면 자동으로 취소된다 

```kotlin
@Composable
fun BoxLayout() {
    val scope = rememberCoroutineScope()
    var job: Job? by remember {
        mutableStateOf(null)
    }

    Column(modifier = Modifier.fillMaxSize()) {
        Button(onClick = {
            println("Timer started")
            job = scope.launch {
                try {
                    delay(5000L)
                    println("Timer ended")
                } catch (e: Exception) {
                    println("Timer cancelled")
                }
            }
        }) {
            Text(text = "Start Timer")
        }
        
        Spacer(Modifier.height(20.dp))
        
        Button(onClick = {
            println("Cancelling timer")
            job?.cancel()
        }) {
            Text(text = "Cancel Timer")
        }
    }
}
```

- 이런식으로 job을 반환받아서 취소하면 하나의 job만 취소됨. -> timer를 여러개 클릭할경우 다른건 취소가 안 된다

```kotlin
@Composable
fun BoxLayout() {
    val scope = rememberCoroutineScope()

    Column(modifier = Modifier.fillMaxSize()) {
        Button(onClick = {
            println("Timer started")
            scope.launch {
                try {
                    delay(5000L)
                    println("Timer ended")
                } catch (e: Exception) {
                    println("Timer cancelled")
                }
            }
        }) {
            Text(text = "Start Timer")
        }
        
        Spacer(Modifier.height(20.dp))
        
        Button(onClick = {
            println("Cancelling timer")
            scope.cancel()
        }) {
            Text(text = "Cancel Timer")
        }
    }
}
```

- 스코프를 취소해버리면 다 날라감



#### 3. rememberUpdatedState

##### code

```kotlin
@Composable
fun BoxLayout() {
    var buttonColor by remember {
        mutableStateOf("Unknown")
    }
    Column {
        Button(
            onClick = {
                buttonColor = "Red"
            },
            colors = ButtonDefaults.buttonColors(
                backgroundColor = Color.Red
            )
        ) {
            Text("Red Button")
        }

        Spacer(Modifier.height(24.dp))

        Button(
            onClick = {
                buttonColor = "Black"
            },
            colors = ButtonDefaults.buttonColors(
                backgroundColor = Color.Black
            )
        ) {
            Text("Black Button")
        }

        Timer(buttonColor = buttonColor)
    }
}


@Composable
fun Timer(
    buttonColor: String
) {
    val timerDuration = 5000L
    println("Composing timer with colour : $buttonColor")
    val buttonColorUpdated by rememberUpdatedState(newValue = buttonColor)
  // == var buttonColorUpdated by remember { mutableStateOf(buttonColor) }
  //    buttonColorUpdated = buttonColor

    LaunchedEffect(key1 = Unit, block = {
        startTimer(timerDuration) {
            println("Timer ended Last pressed button color was $buttonColor")
            println("Timer ended Last pressed button color was $buttonColorUpdated")
        }
    })
}

suspend fun startTimer(time: Long, onTimerEnd: () -> Unit) {
    delay(timeMillis = time)
    onTimerEnd()
}
```

- 기존 인자로받은 buttonColor는 중간에 값이 바뀌어도 리컴포지션이 일어나지 않기때문에 그대로 unknown이 호출된다.
- rememberUpdatedState를 사용하면 바뀐 값을 사용할 수 있음

