# kotlin-snippets

## Null Safety
```kotlin
var b: String? = "abc"
b = null // ok
print(b)


var b: String? = null
if (b != null && b.isNotEmpty()) {
    print(b)
}

if (b?.isNotEmpty() == true){
  print(b)
}

bob?.department?.head?.name

val listWithNulls: List<String?> = listOf("Kotlin", null)
for (item in listWithNulls) {
    item?.let { println(it) } // prints A and ignores null
}


//elvis
val l = b?.length ?: -1

//early return
myVar ?: return

//for NPE-lovers
val l = b!!.length
```

## String templates
```kotlin
// simple name in template:
var a = 1
val s1 = "a is $a" 
a = 2
// arbitrary expression in template:
val s2 = "${s1.replace("is", "was")}, but now is $a"
````

## Using if as an expression:
```kotlin
fun maxOf(a: Int, b: Int) = if (a > b) a else b
```

## Using type checks and automatic casts
```kotlin
if (obj is String) {
        // `obj` is automatically cast to `String` in this branch
        return obj.length
    }
```

## Using a for loop
```kotlin
val items = listOf("apple", "banana", "kiwifruit")
for (item in items) {
    println(item)
 }
 ```
 
## Using when expression
```kotlin
fun describe(obj: Any): String =
  when (obj) {
     1          -> "One"
     "Hello"    -> "Greeting"
      is Long    -> "Long"
      !is String -> "Not a string"
      else       -> "Unknown"
  }
```
    
 
## Ranges
```kotlin
for (x in 1..5) {
    print(x)
}
```

## Collections
```kotlin
when {
    "orange" in items -> println("juicy")
    "apple" in items -> println("apple is fine too")
}
```

Prefer using `when` if there are three or more options.

## Lambdas
```kotlin
val fruits = listOf("banana", "avocado", "apple", "kiwifruit")
fruits
  .filter { it.startsWith("a") }
  .sortedBy { it }
  .map { it.toUpperCase() }
  .forEach { println(it) }
  
  val numbers = listOf(1, 6, 2, 7, 8, 9)
  numbers.find { it > 3 } // 6
  numbers.findLast{it > 3} // 9
  numbers.any{it > 8} // true
```

  
## Default function values
```kotlin
fun foo(a: Int = 0, b: String = "") { ... }
```

## Lists
```kotlin
val list = listOf("a", "b", "c")
val mutableList = arrayListOf("a", "b", "c")
```

## Read-only map
```kotlin
val map = mapOf("a" to 1, "b" to 2, "c" to 3)
```

## Lazy property
```kotlin
val preference: String by lazy {
  sharedPreferences.getString(PREFERENCE_KEY) 
}
```

## Constructors
```kotlin
class Person(firstName: String) { ... }

class Customer public @Inject constructor(name: String) { ... }

// secondary constructor
class Person {
    constructor(parent: Person) {
        parent.children.add(this)
    }
}

class Constructors {
    init {
        println("Init block") // first
    }

    constructor(i: Int) {
        println("Constructor") // second
    }
}
```

## Getter setter
```kotlin
var email:String = ""
set(value) {
    if (value.isValidEmail()){
        field = value
    }
}
```

## Late initialization
```kotlin
class MyActivity : AppCompatActivity() {
  // non-null, but not initalized
  lateinit var recyclerView: RecyclerView

  override fun onCreate(savedInstanceState: Bundle?) {
    // …
    // initialized here
    recyclerView = findViewById(R.id.recycler_view)
  }
}
```

## Inline functions
Calls will be replaced with the function body.
```kotlin
// define an inline function that takes a function argument
inline fun onlyIf(check: Boolean, operation: () -> Unit) {
  if (check) {
    operation()
  }
}
// call it like this
onlyIf(shouldPrint) { // call: pass operation as a lambda
  println(“Hello, Kotlin”)
}
// which will be inlined to this
if (shouldPrint) { // execution: no need to create lambda
  println(“Hello, Kotlin”)
}
```

## Extensions
```kotlin
fun View.hide() {
    visibility = View.GONE
}

textView.hide()

fun MutableList<Int>.swap(index1: Int, index2: Int) {
    val tmp = this[index1] // 'this' corresponds to the list
    this[index1] = this[index2]
    this[index2] = tmp
}
  
val l = mutableListOf(1, 2, 3)
l.swap(0, 2) // 'this' inside 'swap()' will hold the value of 'l'
```

## Data classes
```kotlin
data class User(val name: String = "", val age: Int = 0)

data class Person(val name: String) {
    var age: Int = 0 // excluded
}

//destructing
val jane = User("Jane", 35) 
val (name, age) = jane
println("$name, $age years of age") // prints "Jane, 35 years of age"
```

## Sealed classes
```kotlin
sealed class NetworkResult
data class Success(val result: String): NetworkResult()
data class Failure(val error: Error): NetworkResult()
// one observer for success and failure
viewModel.data.observe(this, Observer<NetworkResult> { data ->
  data ?: return@Observer // skip nulls
  when(data) {
    is Success -> showResult(data.result) // smart cast to Success
    is Failure -> showError(data.error) // smart cast to Failure
  }
})


// use Sealed classes as ViewHolders in a RecyclerViewAdapter
override fun onBindViewHolder(
  holder: SealedAdapterViewHolder?, position: Int) {
  when (holder) { // compiler enforces handling all types
    is HeaderHolder -> {
      holder.displayHeader(items[position]) // smart cast here
    }
    is DetailsHolder -> {
      holder.displayDetails(items[position]) // smart cast here
    }
  }
}
```

## Generics
```kotlin
class ApiResponse<T>(t:T){
    var value = t
}

val r = ApiResponse<String>("Data")
val r = ApiResponse<User>(User("name",20))

```

## Functions
```kotlin
fun reformat(str: String,
             normalizeCase: Boolean = true,
             upperCaseFirstLetter: Boolean = true,
             divideByCamelHumps: Boolean = false,
             wordSeparator: Char = ' ') {
...
}

reformat(str)

reformat(str, true, true, false, '_')

reformat(str,
    normalizeCase = true,
    upperCaseFirstLetter = true,
    divideByCamelHumps = false,
    wordSeparator = '_'
)
```

```kotlin
fun <T> asList(vararg ts: T): List<T> {
    val result = ArrayList<T>()
    for (t in ts) // ts is an Array
        result.add(t)
    return result
}

val list = asList(1, 2, 3)
```

## infix notation
```kotlin
infix fun Int.shl(x: Int): Int { ... }

// calling the function using the infix notation
1 shl 2

// is the same as
1.shl(2)
```

## Local functions
```kotlin
fun dfs(graph: Graph) {
    fun dfs(current: Vertex, visited: Set<Vertex>) {
        if (!visited.add(current)) return
        for (v in current.neighbors)
            dfs(v, visited)
    }

    dfs(graph.vertices[0], HashSet())
}
```

## apply, also, run, let
```kotlin
val string = "a"
val result = string.apply {
    // this = "a"
    // it = not available
    4 // Block return value unused
    // result = "a"
}

val string = "a"
val result = string.also {
    // this = this@MyClass
    // it = "a"
    4 // Block return value unused
    // result = "a"
}


val string = "a"
val result = string.run {
	// this = "a"
	// it = not available
	1 // Block return value
	// result = 1
}

val string = "a"
val result = string.let {
    // this = this@MyClass
    // it = "a"
    2 // Block return value
    // result = 2
}
```

## Higher order functions
```kotlin
fun <T> ArrayList<T>.filterOnCondition(condition: (T) -> Boolean): ArrayList<T>{
    val result = arrayListOf<T>()
    for (item in this){
        if (condition(item)){
            result.add(item)
        }
    }

    return result
}
...
fun isMultipleOf (number: Int, multipleOf : Int): Boolean{
    return number % multipleOf == 0
}
...
var list = arrayListOf<Int>()
for (number in 1..10){
    list.add(number)
}
var resultList = list.filterOnCondition { isMultipleOf(it, 5) }
```

## Coroutines

```kotlin
GlobalScope.launch {
    delay(2000) // Suspend function waits until completed
    log("one")
    delay(1000)
    log("two")
}
log("three")

/*
Output will be
0ms >    three
2000ms > one
3000ms > two
*/
```

```kotlin
runBlocking{
    val job = GlobalScope.launch {
        delay(2000)
        log("one")
    }
    log("two")
    job.join() // Job starts here waits until completed
    log("three")
}

/*
Output will be
0ms >    two
2000ms > one
2000ms > three
*/
```

```kotlin
runBlocking {
    log("zero")
    launch { // launch block starts but doesn't wait to be completed. Because launch block is not suspend function
        delay(200)
        log("one")
    }

    launch {
        delay(300)
        log("two")
    }
    coroutineScope { // this block starts but waits for its all inner blocks until complete to process to the next line
        launch {
            delay(1000)
            log("three")
        }

        delay(500)
        log("four")
    }
    delay(1000)
    log("five")
}
/*
Output will be
0ms > 	 zero
200ms >  one
300ms >  two
500ms >  four
1000ms > three
2000ms > five
*/
```
Simple Presenter-Interactor Retrofit service call
```kotlin
class MyPresenter(private val view: BaseView) {

    private val job = Job()
    private val dispatcher = Dispatchers.Main
    private val uiScope = CoroutineScope(dispatcher + job)
    val interactor = MyInteractor()

    fun getResult() {
        view.showLoading()

        uiScope.launch {
            val response = interactor.getResult()
            response.response?.apply {
                view.onGetResult(result)
            }
            response.error?.apply { view.onError(this) }
            view.hideLoading()
        }
    }
}

class MyInteractor(private val service: DummyService) {

    suspend fun getResult(): ApiResponse<ResultResponse> {
        return service.getResult().defer()
    }
    
}
```


## What's next?
[Kotlin koans](https://kotlinlang.org/docs/tutorials/koans.html)

[Coroutines](https://kotlinlang.org/docs/tutorials/coroutines-basic-jvm.html)

[Androidx](https://developer.android.com/topic/libraries/support-library/androidx-overview)

[Try Kotlin Online](https://try.kotlinlang.org)



## Resources
https://kotlinlang.org/

https://medium.com/google-developers/31daysofkotlin-week-1-recap-fbd5a622ef86

https://medium.com/google-developers/31daysofkotlin-week-2-recap-9eedcd18ef8

https://medium.com/google-developers/31daysofkotlin-week-3-recap-20b20ca9e205

https://medium.com/google-developers/31daysofkotlin-week-4-recap-d820089f8090

https://medium.com/@agrawalsuneet/higher-order-functions-in-kotlin-3d633a86f3d7
