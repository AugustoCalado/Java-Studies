
# Java - Exception and Custom Exception

## Java Exception Structure - Overview 
* Checked Exceptions
	*	Checked Exception (`Exception`) **extends** `Throwable`
* Unchecked Exceptions
	* Unchecked Exceptions (`RuntimeException`) **extends** `Exception`.
* Errors 
	* Example of typical error StackOverflowError 
	* `Error` **extends** `Throwable`

## Rethrowing an Exception Wrapped Inside a Custom Exception
It is sometimes necessary to catch an exception and re-throw it by adding in a few more details. This is **usually common** if you have various **error codes defined** in your application that needs to be either logged or returned to the client in case of that particular exception.

**TIP:** It is always a good practice to capture the root cause of the exception hence the **Throwable** argument which can be passed` to the parent class constructor.

```java
public  class InvalidCurrencyDataException extends RuntimeException {
	private Integer errorCode; public InvalidCurrencyDataException(String message) { 
		super(message); 
	} 
	public InvalidCurrencyDataException(String message, Throwable cause) { 
		super(message, cause); 
	}
	public InvalidCurrencyDataException(String message, Throwable cause, ErrorCodes errorCode) { 
		super(message, cause);
		this.errorCode = errorCode.getCode(); 
	} 
	public Integer getErrorCode() { 
		  return errorCode; 
	} 
  }
```
```java
public class CurrencyService {  
    public String convertDollarsToEuros(String value) {
        try {
	        int x = Integer.parseInt(value);
	    } catch (NumberFormatException e) {
	        throw new InvalidCurrencyDataException("Invalid data", e, ErrorCodes.VALIDATION_PARSE_ERROR);
        }
        return value;
    }
}

In the above code, NumberFormatException is captured and rethrown insede the custom exception `InvalidCurrencyDataException`
```


##  try-with-resources Statement
```java
public String readFirstLine(String path) throws IOException {
    BufferedReader br = new BufferedReader(new FileReader(path));   
    try {
        return br.readLine();
    } finally {
        if(br != null) br.close();
    }
}
```
The above code can be written in a simpler way, as follows:
```java
static String readFirstLineFromFile(String path) throws IOException {
    try(BufferedReader br = new BufferedReader(new FileReader(path))) {
        return br.readLine();
    }
}
```
## Best Exception Handling Practices

1. Avoid Exceptional Conditions - by using simple checks, it is possible to avoid throwing an exception.
2. Use try-with-resources - Give the preference in the try-with-resources to work with resources.
3. avoid use **swallowing the exception**(when an exception is caught and the issue is not fixed)
4. avoid return in finally blocks - by return something in the `finally` block all the data generated by the exception thrown is lost
5. avoid throwing in a `finally` block - it will drop the exception from the try-catch block
6. avoid logging and throwing - this practice is redundant and only result in a bunch of log messages
7. avoid create a catch block only with `Exception ex` - By doing so, checked and runtime exceptions will caught and as Runtime Exceptions represent problems directly related with programming problems, it is really hard to predict a reasonably catch block for handle them.


## References
[exception-handling-in-java-a-complete-guide-with-best-and-worst-practices](https://stackabuse.com/exception-handling-in-java-a-complete-guide-with-best-and-worst-practices/)
