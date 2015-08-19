# Introduction #

Using the PHP-SIP2 library is fairly straightforward.  Include the class in your PHP file, create a new object, populate the settings, then request data.  This example works against a server that does not require a login step. It is trivial to add if needed by your ILS.


# Details #

```
<?php
/* Sample code to test the SIP2 library
*
* Author: John Wohlers 
*
*/

// Include class library 
include('php-sip2/sip2.class.php');

// create object
$mysip = new sip2;

// Set host name

$mysip->hostname = 'server.example.com';

$mysip->port = 6002;

// connect to SIP server 
$result = $mysip->connect();

//send selfcheck status message
$in = $mysip->msgSCStatus();
$result = $mysip->parseACSStatusResponse($mysip->get_message($in));

/*  Use result to populate SIP2 setings 
*   (In the real world, you should check for an actual value 
*   before trying to use it... but this is a simple example)
*/
$mysip->AO = $result['variable']['AO'][0]; /* set AO to value returned */
$mysip->AN = $result['variable']['AN'][0]; /* set AN to value returned */

// Identify a patron
$mysip->patron = '101010101';
$mysip->patronpwd = '010101';

// Get Charged Items Raw response
$in = $mysip->msgPatronInformation('charged');

// parse the raw response into an array
$result = $mysip->parsePatronInfoResponse( $mysip->get_message($in) );

// Display Array contents
print_r ($result);
?>
```