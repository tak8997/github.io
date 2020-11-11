# Coroutine

### Coroutine이란?

co + routine => cooperative + function
하나의 쓰레드 안에서 함수들이 서로 협력한다.
Coroutine 은 Thread 의 대안이 아니라 기존의 Thread를 더 잘게 쪼개어 사용하기위한 개념임.

Coroutines allow you to achieve concurrent behavior without switching threads, which results in more efficient code. Therefore, coroutines are often called “lightweight threads”.

it is possible to achieve concurrency without creating expensive threads

A framework to manage concurrency in a more performant and simple way with its lightweight thread which is written on top of the actual threading framework to get the most out of it by taking the advantage of cooperative nature of functions.


### Structured Concurruncy

nested 코루틴 스코프를 관리하는 기법. 코루틴 안에 또 다른 코루틴 스코프를 만들어서 관리하는 기법.
outer coroutine does not complete until all the coroutines launched in its scope complete. 
https://kotlinlang.org/docs/reference/coroutines/basics.html#structured-concurrency

1. Cancel work when it is no longer needed. ->  When a scope cancels, all of its coroutines cancel
2. Keep track of work while it’s running. ->  When a suspend fun returns, all of its work is done.
3. Signal errors when a coroutine fails. -> When a coroutine errors, its caller or scope is notified.


### coroutineScope, supervisorScope 

coroutineScope and supervisorScope will wait for child coroutines to complete.
coroutineScope, supervisorScope 차이 : child코루틴 들 간에 에러 전파 여부.
coroutineScope는 에러 던져지면 즉시 child, parent 모두 전파.
supervisorScope는 에러 던져지면 child들에게만 전파(단, 다른 코루틴이 끝날때까지 대기). 부모는 cancecl안됨.
https://tourspace.tistory.com/155
https://blog.mindorks.com/mastering-kotlin-coroutines-in-android-step-by-step-guide
https://stackoverflow.com/questions/53577907/when-to-use-coroutinescope-vs-supervisorscope
https://proandroiddev.com/managing-exceptions-in-nested-coroutine-scopes-9f23fd85e61


### Coroutine Scope, Coroutine Context

CoroutineScope -
CoroutineScope는 coroutineContext를 가지고있는 interface이다.
all coroutines run inside a CoroutineScope. A scope controls the lifetime of coroutines through its job -> coroutine 상태 추적
https://medium.com/androiddevelopers/coroutines-on-android-part-ii-getting-started-3bff117176dd

There is also an interface called CoroutineScope that consists of a sole property — val coroutineContext: CoroutineContext. It has nothing else but a context. So, why it exists and how is it different from a context itself? The difference between a context and a scope is in their intended purpose.
->Every coroutine builder is an extension on CoroutineScope and inherits its coroutineContext to automatically propagate both context elements and cancellation.
즉, 스코프를 자동으로 전파하려고. 스코프 hierarchy만들려고. 결국, job을 통해 cancel.
https://medium.com/@elizarov/coroutine-context-and-scope-c8b255d59055
https://stackoverflow.com/questions/54416840/kotlin-coroutines-scope-vs-coroutine-context
https://proandroiddev.com/demystifying-coroutinecontext-1ce5b68407ad


### Job
A coroutine itself is represented by a Job. It is responsible for coroutine’s lifecycle, cancellation, and parent-child relations. A current job can be retrieved from a current coroutine’s context:
