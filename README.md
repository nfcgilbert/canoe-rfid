## How to implement RFID pos comms in Canoe
 
1. Install Cordova HCE with
> cordova plugin add cordova-plugin-hce --variable AID_FILTER=F2A731D87C 
2. For some reason that plugin insists on sending an even number of bytes. The rfid protocol of the pos wasn't made for that, so we need to make a modification to the plugin. Replace HCEPlugin.Java with the uploaded file (/platforms/etc), now it allows for uneven number of bytes. Afaik this must be done after first building for Android. 
3. The files from above do the following:
     - Implement 2 new views and related controllers. rfid-status is to inform the user about errors/outcome, rfid-invoice displays the invoice.
     - Implement rfid logic.
 
### About the code
 
1. The account that's used for rfid payments is atm always index 0. 
2. I use global variables to communicate between rfid logic (mainly RFIDCardService) and controllers. Both welcomeController.js and nanoService.js do window.global_state=$state, which is the $state param taken from the controller, so that the CardService can use it to change the views. This was the best way I could currently think of. Maybe someone can find a better way. I'm not that deep into Angular. Also perhaps it's not so bad/necessary? 
3. The information for the status and invoice views is also stored in global variables such as window.rfid_status_headline, window.rfid_status_message, or window.rfid_invoice_xxx. 
4. I implemented a variable window.rfid_wallet_locked which should be true or false depending on whether the wallet is locked. The rfid comms should not work when the wallet is locked.
 
### Known issues
 
1. I haven't finished the optical design - or even really started. You probably know better how you want this to look anyway. 
2. Probably due to the way I implemented this, the UI changes can occur slightly delayed. A few apply()s could perhaps improve things.
3. Some variables could have better names.
