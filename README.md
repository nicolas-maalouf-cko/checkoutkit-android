### Requirements

Java 1.5 and later

### How to use the library

Create an Android project. Go to Module settings and add ----com.checkout:checkoutkit:1.0 ??---- as a library dependency (from Maven). compile 'com.checkout:checkoutkit:1.0'

### Example

Import the **CheckoutKit.java** in your code as below:
```
import com.checkout.CheckoutKit;
```

You will need to specify at least the public key when creating an instance of ***CheckoutKit***. We offer a wide range of constructors, the ***CheckoutKit*** instance is entirely customizable:

```html
CheckoutKit.getInstance(String publicKey)
CheckoutKit.getInstance(String publicKey, Environment baseUrl)
CheckoutKit.getInstance(String publicKey, boolean logging)
CheckoutKit.getInstance(String publicKey, Environment baseUrl, boolean logging)
CheckoutKit.getInstance(String publicKey, boolean logging, PrintStream logger)
CheckoutKit.getInstance(String publicKey, Environment baseUrl, boolean logging, PrintStream logger)
CheckoutKit.getInstance(String publicKey, Environment baseUrl, PrintStream logger)
CheckoutKit getInstance(String publicKey, PrintStream logger)
CheckoutKit getInstance(String publicKey, boolean logging, String file)
CheckoutKit getInstance(String publicKey, Environment baseUrl, boolean logging,  String file)
CheckoutKit getInstance(String publicKey, Environment baseUrl, String file)
CheckoutKit getInstance(String publicKey, String file)
CheckoutKit getInstance() // to use only once the CheckoutKit object has been instantiated, otherwise returns null
```

These functions can throw CheckoutException (when the public key specified is invalid) or IOException (when the logging file cannot be accessed or written to).
Here are more details about the parameters :
- publicKey : String containing the public key. It is tested against the following regular expression : ^pk_(?:test_)?(?:\w{8})-(?:\w{4})-(?:\w{4})-(?:\w{4})-(?:\w{12})$. Mandatory otherwise the ***CheckoutKit*** object cannot be instantiated.
- baseUrl : Environment object containing the information of the merchant's environment, default is SANDBOX. Optional.
- logging : boolean, if the debug mode is activated or not, default is true. Optional.
- logger : PrintStream object containing a stream to print to the desired logging system, default is the console (i.e. System.out). Optional.
- file : String containing the log file name, if the file does not exist, it is created, otherwise log entries are appended. Optional.

If **logging** is set to true, many actions will be traced to the logging system (either a file or the console).

The available functions of ***CheckoutKit*** can be found in the Javadoc : JAVADOC LINK.

Another class is available for utility functions: ***CardValidator***. It provides static functions for validating all the card related information before processing them. The list of the available functions is available in the Javadoc : JAVADOC LINK.


**Create card token**

```
public class Example {

    private String publicKey = "your_public_key";

    public void main(String[] args) {

        try {

        	/* Take the card information where ever you want (form, fields in the application page...) */
            Card card = new Card(number.getText().toString(), name.getText().toString(), expMonth.getText().toString(), expYear.getText().toString(), cvv.getText().toString());

            /* Instantiates the CheckoutKit object with the minimum parameters, default configuration for the other ones */
            CheckoutKit ck = CheckoutKit.getInstance(publicKey);

            /* Makes the call to the server (specified in environment variable) */
            Response<CardTokenResponse> resp = ck.createCardToken(card);

            if (resp.hasError) {
                /* Print error message or handle it */
            } else {
                /* The field containing the actual card token */
                CardToken ct = resp.model.getCard();
            }

        } catch (CardException e) {
        	/* Happens when the card informations entered by the user are not correct. Display error message or log the error */
        	/* Type of the exception in the enum CardExceptionType. Different types are INVALID_CVV (if the CVV does not have the correct format), INVALID_EXPIRY_DATE (if the card is expired), INVALID_NUMBER (if the card's number is wrong). */
        }
    }
}
```

### Logging
TODO
Logging entries can be added with the function log(String message, LoggerLevel level) available for the ***CheckoutKit*** instance. The level can have the following values : LOW, MEDIUM or HIGH. The printing format is: current date (yyyy/MM/dd HH:mm:ss) - level - message. This is printed to the logger specified in the ***CheckoutKit*** instance (default is the console). The logger can be specified via the functions setLogger or getInstance. The log entries are then added to the logger specified.

### Unit Tests

All the unit test written with JUnit (v4) and resides in the test package.