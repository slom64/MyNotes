
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