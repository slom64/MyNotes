
## KDC

It supplies tickets and generate temporary session keys. 
KDC Stores all symmetric keys for both users and services.

It contain 2 servers inside KDC: ==authentication server== `AS` , ==ticket granting server== `TGS`

### Authentication server
confirms that known user is making access request and issue `TGT` 

### Ticket Granting Server
Confirms that a user is making an access reques to known service, and issue `ST` service tickets. `TGS` 


[[Z Assets/Images/Pasted image 20250817212426.png]]
![[Z Assets/Images/Pasted image 20250817212426.png]]

Before start, there is 2 types of messages:
- `authenticators`: used to authenticate the user to KDC, and to authenticate service to user "mutual authentication".
- `tickets`


> [!NOTE] 
> When we say TGS that doesn't mean we refer to TGS ticket in this context. but we refer to "**Ticket Granting Service**" 


# Steps
Mainly there is 3 important phases sent from user:

## phase 1
- user send `unencrypted` message to `authentication service`, contain `userID` and `serviceID`
- `Authentication service` looks at `userID`, Authentication service has list of all users and their sercert keys. So, it check if userID in the list, if so it graps the `user secret key`.
- Authentication service create 2 messages then send them to user:
	- First message contain `TGSName/ID`, timestamp, lifetime, and `TGS session key`.                                                                                               🔒All encrypted using `user secret key`.
	- Second message `TGT` *ticket granting ticket* contain `UserID`, `TGSName/ID`, TimeStamp, UserIP, TGT lifetime, `TGS session key`.    🔒All ecrypted using `TGS secret key`
- The user decrypt the first message using `user secret key`, 
	- `user secret key` is generated using user password, added to it username@realm.com as salt. then we get the hash. This hash is the 🔓 `user secret key`.
	- This is the first step of validating users password, because if we have wrong user password, we won't be able to decrypt the message.
	- Now we have `TGS session key`, which will be very helpful.

> [!NOTE] **Ticket granting ticket decryption**
> **User can't decrypt TGT *'ticket granting ticket'* because he doesn't has TGS secret key**.

## phase 2
- user send 3 messages:
	- first message:`TGT`                                                                                                               🔒ecrypted using `TGS secret key`
	- second message contain `ServiceName/ID`, Requested lifetime of ticket.      🔓Unecrypted
	- Thired message:`User authenticator`, which contain `UserID`, timestamp. 🔒encrypted using `TGS session key`
		- Doing this, we make sure no one has intercepted TGT on the wire then relay it again, because if someone did it they won't know the `TGS session key`.
- `Ticket granting server` starts by looking at `serviceName/ID` in the unecrypted message, if its not found the request is denyed. if found then `TGS` grap `service secret key`.
- Then 🔓 decrypte `TGT` using `TGS secret key`, So we get `TGS session key` that will be used to 🔓decrypte `User authenticator`.
- Now everything is unencrypted. The TGS start to validate data:
	- check if `UserID` in `TGT` and `User authenticator` match each other. And TimeStamp is checked too.
	- Compare the IP address in TGT with the IP that is currently requesting.
	- check TGT has not expired.
- If all is good, then `TGS` use `TGS cache` to cache recently used `User authenticator`. And TGS make sure `User authenticator` isn't already in the cache. protection from replay.
- The TGS starts to create 2 messages and create random symmetric `service session key`:
	- First message: `ServiceID/Name` that the user want, TimeStamp, Lifetime, `service session key`.                                                                        🔒ecrypted using `TGS session key`
	- second message is`ST` `'service ticket'`: it contain `userID`, `ServiceID/Name`, timestamp, User IP, lifetime, `service session key`. 🔒encrypted by`service session key`
- User 🔓decrypte the first message using `TGS session key`
	- now user has a copy of `serive session key`.
	- User can't decrypte `ST`, because he doesn't has `service secret key`.

## phase 3
- user send 2 messages:
	- first message: `ST` `'service ticket'`
	- second message `User authenticator`