# STM32-MySQL mbed
MySQL Client for STM32 using [Mbed library](https://github.com/ARMmbed/mbed-os)

# How to use this code ?
## Hardware
![alt text](https://os.mbed.com/media/cache/platforms/DISCO_F746NG.jpg.250x250_q85.jpg)

It can be executed on every target with Ethernet/WiFi.
This code has been tested on the [32F746GDISCOVERY](https://www.st.com/en/evaluation-tools/32f746gdiscovery.html)
## IDE
### IDE Used
To compile and flash this code I used [Mbed Studio](https://os.mbed.com/studio/)
### Implement this code
- Add the library to your mbed project (Right click on you project folder, and select **Add Library...**)
- Setup your network connection
- Create a TCP socket and initialize it
- Create a MySQL object by passing the initialized socket into the constructor argument

# main.c Exeample
## Code
```C++
#include "mbed.h"
#include "EthernetInterface.h"
#include "STM32_MySQL.h"

int main(void){
	const char* device_ip =	"Your STM32 IP";
	const char* server_ip =	"Your MySQL Server IP";
	const char* gateway =	"Your Gateway IP";
	const char* netmask =	"Your Netmask";

	char* mysql_user = 	"Your Username";
	char* mysql_password = 	"Your Password";

	EthernetInterface eth;

	//Network Configuration
	eth.set_dhcp(false);
	eth.set_network(device_ip, netmask, gateway);
	eth.connect();

	//MySQL Object declaration
	MySQL sql(&eth, server_ip);

	sql.connect(mysql_user, mysql_password);

	TypeDef_Database* Database = NULL;

	while(true){
		//Get Table from Database
		Database = sql.query("SELECT * FROM database.table;",Database);

		//Print database over serial if something has been received
		if(Database!=NULL) sql.printDatabase(Database);

		//Make a simple query, if the query is OK, it returns true
		if(sql.query("INSERT INTO database.table VALUES(Your values);")){
			printf("Query OK !\r\n");
		}

		//Sleep for 5 seconds
		ThisThread::sleep_for(5s);
	}
}
```
## Edit
### Configure the constants
```C++
const char* device_ip =	"Your STM32 IP";
const char* server_ip =	"Your MySQL Server IP";
const char* gateway =	"Your Gateway IP";
const char* netmask =	"Your Netmask";

char* mysql_user = 	"Your Username";
char* mysql_password = 	"Your Password";
```
### Run query
```C++
Database = sql.query("SELECT * FROM database.table;",Database);
```
```C++
sql.query("INSERT INTO database.table VALUES(Your values);")
```
