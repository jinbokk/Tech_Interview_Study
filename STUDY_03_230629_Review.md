# 이진복

- Javascript는 싱글 스레드 기반의 언어인데, 어떻게 비동기 작업이 가능한걸까? 다시 말하자면, 하나의 main함수가 있다고 가정했을때,
동기적인 함수와 비동기적인 함수가 순차적으로 실행되는 경우를 예를 들자면, javascript는 스레드가 하나이고, 스택도 하나인데, 비동기 작업은 어디서 처리가 되어
블로킹 현상 없이 main 함수 흐름에 올라 탈 수 있는 걸까? 만일 블로킹이 발생한다면 그건 비동기적인 처리와 거리가 먼 것이 아닌가?


![loupe](https://github.com/jinbokk/tech-interview-study/assets/101123079/a4581e5a-7052-46bc-a95b-03e94e222e68)

    
```
여기서 Event Loop와 Web API의 개념을 알고 있었어야 했다. 위의 이미지를 기억해야 한다.

Javascript는 싱글 스레드 기반으로 하나의 실행 스택(Call Stack)을 가지고 있는것이 맞다.

예를들어, Javascript 파일 안에서 어떠한 main이 되는 함수가 실행되면, 
V8 엔진이 코드를 위에서 아래로 순차적으로 실행해가며 각 단계를 스택에 넣는다.

이때 동기적인 함수나 비동기적인 함수나 구분하지 않고, 순차적으로 스택을 쌓으며 LIFO 순서로 작업을 처리한다.

이때 중요한 포인트가 바로 "동기적인 함수는 동기적으로 실행하지만, 
비동기적인 함수는 Web API를 이용해 브라우저 엔진이 처리할 수 있도록 옮긴다."는 것이다.

Javascript 입장에서는 스택에 있던 비동기 함수를 스택에서 제외하며 실행시킨다.

이때 실행된 비동기 함수는 Web API에서 처리가 되고, 브라우저 엔진이 관리하고 있는 Callback 큐로 이동한다.

이후 Javascript의 실행 스택이 모두 비워져서 더 이상 작업이 없는 상태가 되었을때, 
Javascript의 실행스택과 브라우저의 Callback Que를 모두 주시하고 있는 Event Loop가 동작하며
Callback Que에 있던 콜백 함수를 Javascript의 Stack으로 이동시키고, 

마지막으로 Javascript는 본인의 Stack에 있던 콜백 함수까지 실행하며 모든 작업이 끝나게 되는 것이다.
```

```
추가적으로, 만일 timeout이 1초로 동일한 비동기 함수가 3개가 실행된다면 어떻게 될까?
이러한 경우에는 
```


[(참고 강좌) What the heck is the event loop anyway? | Philip Roberts | JSConf EU](https://www.youtube.com/watch?v=8aGhZQkoFbQ)

[(과정 비주얼화 웹 앱) latentflip.com/loupe](http://latentflip.com/loupe/?code=JC5vbignYnV0dG9uJywgJ2NsaWNrJywgZnVuY3Rpb24gb25DbGljaygpIHsKICAgIHNldFRpbWVvdXQoZnVuY3Rpb24gdGltZXIoKSB7CiAgICAgICAgY29uc29sZS5sb2coJ1lvdSBjbGlja2VkIHRoZSBidXR0b24hJyk7ICAgIAogICAgfSwgMjAwMCk7Cn0pOwoKY29uc29sZS5sb2coIkhpISIpOwoKc2V0VGltZW91dChmdW5jdGlvbiB0aW1lb3V0KCkgewogICAgY29uc29sZS5sb2coIkNsaWNrIHRoZSBidXR0b24hIik7Cn0sIDUwMDApOwoKY29uc29sZS5sb2coIldlbGNvbWUgdG8gbG91cGUuIik7!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D)
