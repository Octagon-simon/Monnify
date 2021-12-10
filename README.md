### ABOUT

This is a Simple Monnify Class that can be used to Initialize a Transaction and Verify the status of the transaction.
The class will generate an Access token using the api key and secret key you have provided and you can modify the class my adding more endpoints that would need the access token or basic Authentication.

#### Initialize Transaction

Before you proceed, make sure you have provided your Api key and Secret Keys as constants.

```
<?php

//Your Data Here
DEFINE ('API_KEY', 'YOUR_API_KEY_HERE');
DEFINE ('SECRET', 'YOUR_SECRET_KEY_HERE');

?>

``` 
Then create an array of data needed for initializing the transaction, and pass that array as a parameter to the method.

```
<?php

//require the class

require_once monnify/monnify.php

//Instantiate the class

$monnify = new Monnify();

$data= array(
 	"amount"=> 100,
  	"customerName"=> "Simon Ugorji",
  	"customerEmail"=> "test@tester.com",
  	"paymentReference" => "12345446789",
  	"paymentDescription"=> "GMAS Premium Membership",
  	"currencyCode"=> "NGN",
  	"contractCode"=> "123456789",
  	"redirectUrl"=> "https://www.mycallbackurl.com",
  	"paymentMethods"=> [
    "CARD",
    "ACCOUNT_TRANSFER"
  ]);

//Initialize transaction and redirect to checkout url

$init = $monnify->initTrans($data);

if($init['checkoutUrl']){

	$url = $init['checkoutUrl'];
	header("Location: $url");
}else{
	//handle error here
}
?>

``` 

#### Verify Transaction

By default, your payment reference gets included in the query parameter of your callback Url.
Query the transaction reference form the DB using the payment reference, and the call the verify transaction method.
```
<?php

(isset($_GET) && isset($_GET['paymentReference'])) ? 
( $ref = htmlspecialchars($_GET['paymentReference']) ) : $ref = NULL;

if($ref){

	//Query the transaction reference from your DB call the method

	$verify = $monnify->verifyTrans($trans_ref);

	if($verify['paymentStatus'] == 'PAID'){

		//Payment has been verified!

	}else{
		echo "Transaction Verification Failed!";
		exit();
	}

}else{
	echo "payment Reference is Required!";
	exit();
}


?>

```

#### EXTRA

**Docs Available Here.**

[Login Authentication](https://teamapt.atlassian.net/wiki/spaces/MON/pages/212008633/Authentication)

[Initialize transaction](https://teamapt.atlassian.net/wiki/spaces/MON/pages/213909259/Initialize+Transaction
)

[Verify Transaction Deprecated](https://teamapt.atlassian.net/wiki/spaces/MON/pages/212008662/Get+Transaction+Status+Deprecated)

[Verify Transaction Current](https://teamapt.atlassian.net/wiki/spaces/MON/pages/212008662/Get+Transaction+Status)

```
<?php	
/*
	//sample login responseBody

 	[accessToken] => string()
 	[expiresIn] => int()
 
	//sample Init responseBody

	["transactionReference"]=> string() 
	["paymentReference"]=> string() 
	["merchantName"]=> string() 
	["apiKey"]=> string() 
	["redirectUrl"]=> string() 
	["enabledPaymentMethod"]=> array() { [0]=> string(4) "CARD" [1]=> string(16) "ACCOUNT_TRANSFER" }
	["checkoutUrl"]=> string() 

	//sample verify transaction deprecated response body

 	["paymentMethod"]=> string 
 	["createdOn"]=> string() 
 	["amount"]=> float() 
 	["fee"]=> float() 
 	["currencyCode"]=> string()
 	["completedOn"]=> string() 
 	["customerName"]=> string() 
 	["customerEmail"]=> string() 
 	["paymentDescription"]=> string() 
 	["paymentStatus"]=> string() "PAID" 
 	["transactionReference"]=> string(29) "MNFY|04|20211208110651|001547"
 	["paymentReference"]=> string(13) "Sim1638958072"
 	["payableAmount"]=> float(8500)
 	["amountPaid"]=> float(8500)
 	["completed"] => bool()

 	//sample verify transaction current response body

  	 ["transactionReference"]=> string()
 	 ["paymentReference"]=> string()
 	 ["amountPaid"]=> string() "8500.00"
    ["totalPayable"]=> string() "8500.00"
    ["settlementAmount"]=> string() "8490.00"
    ["paidOn"]=> string() "08/12/2021 11:07:22 AM"
    ["paymentStatus"]=> string() "PAID"
    ["paymentDescription"]=> string()
    ["currency"]=> string() "NGN"
    ["paymentMethod"]=> string() "CARD"
    ["product"]=> array(2) { 
       ["type"]=> string(7) "WEB_SDK"
       ["reference"]=> string(13) "Sim1638958072"
    } 
    ["cardDetails"]=> array() { 
       ["cardType"]=> NULL
       ["authorizationCode"]=> NULL
       ["last4"]=> string(4) "7093"
       ["expMonth"]=> string(2) "12"
       ["expYear"]=> string(2) "22"
       ["bin"]=> string(6) "506099"
       ["bankCode"]=> NULL
       ["bankName"]=> NULL
       ["reusable"]=> bool(false)
       ["countryCode"]=> NULL
       ["cardToken"]=> NULL
    } 
    ["accountDetails"]=> NULL
    ["accountPayments"]=> array(0) { }
    ["customer"]=> array(2) {
       ["email"]=> string(19) "ugorji757@gmail.com"
       ["name"]=> string(12) "Simon Okorie"
    } 
    ["metaData"]=> array(0) { }


*/
?>

```
Thank You!

Need More clarification? Visit my blog post about this PHP class on [Medium](https://simon-ugorji.medium.com/simple-php-class-for-integrating-monnify-payment-gateway-3a6b5653fa44)