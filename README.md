# opx-quagga
As requested by the community, this repo will be used to provide a way to compare and contrast performance of current OpenSwitch vs the previous OpenSwitch release (which only supported Quagga). 

This version was compiled at the beginning of Feburary.  Additional optimizations have been added reducing the overall time by ~1.5 seconds and that will be updated over the next month as we begin the next Dell EMC sync cycle to the OPX Base.


As the previous OpenSwitch software was not tested on the Dell S6000, we have listed the CPU information (and frequency) that can be used to scale comparison. 


## Quagga Test Setup Instructions
### Install Quagga
To install quagga on opx, use 'apt-get install quagga' command

### Set up and Access Quagga
1.	 After installing quagga, by default daemons and debian.conf file will be present in /etc/quagga directory
2.	Copy the "vtysh.conf" file from /usr/doc/quagga/examples/ to /etc/quagga directory to access quagga shell
3.	Touch the Quagga configuration file "Quagga.conf" using 'touch Quagga.conf' command in /etc/quagga directory
4.	Enable the daemons in daemons file by changing the daemons from 'no' to 'yes'
5.	Restart quagga service 
6.	Enable the pager using 'export VTYSH_PAGER=more' in bashrc or environment file and source the file.
7.	Execute command 'vtysh' to login to Quagga Shell
8.	After executing the command, it will show the hostname with # prompt.
9.	Execute 'show run' to check the running configuration.

### Performance And Scalability Test:
3 ports from DUT is connected to Traffic generator. Quagga BGP is enabled on two ports conected to Traffic generator. 
Once the txRate and rxRate are equal, time is measured using the formula (No.of packets transmitted - No.of packet rcvd)/PPS where PPS is number of packets per second.
NPU route learning is measured excluding BGP neighborship establishment time.


### Current Performance numbers using Quagga on the S6000 (Intel(R) Atom(TM) CPU S1220 @ 1.60GHz)
 
#### BGP Link Failover  
| Routes  | Convergence Trial 1 | Convergence Trial 2 | Convergence Trial 3 | Average Convergence |
| :-----: | :-----------------: | :-----------------: | :-----------------: | :-----------------: |
| 5000    | 2.7085              | 2.9179              | 2.8354              | 2.8206              |
| 10000   | 5.8972              |5.6505               |5.5277               |5.6918|
|16000|9.2849|9.1974|11.7645|10.0823|
 

#### BGP Link Flap
| Routes  | Convergence Trial 1 | Convergence Trial 2 | Convergence Trial 3 | Average Convergence |
| :-----: | :-----------------: | :-----------------: | :-----------------: | :-----------------: |
|5000|9.5124|9.4456|5.4727|8.1436|
|10000|8.248|9.0899|10.0542|9.1307|
|16000|8.4454|8.4879|8.5099|8.4811|

#### BGP Session Flap 
| Routes  | Convergence Trial 1 | Convergence Trial 2 | Convergence Trial 3 | Average Convergence |
| :-----: | :-----------------: | :-----------------: | :-----------------: | :-----------------: |
|5000|11.9402	|7.7968|	7.0148|	8.9173|
|	10000	|6.6125	|6.7983	|5.2785|	6.2298|
|	16000	|10.6982|8.5761|	7.9519|	9.0754|
 
#### NPU Route learning time: 
##### Note: This is the time taken for the NPU to learn routes excluding BGP convergence time.
|Number of Routes | Number of Paths | Time (in secs) |
| :-------------: | :-------------: | :------------: |
|16000	|1 path	|3.5|
|8000	|4 path	|4.09|
|4000	|8 path	|3.5|
|4000	|16 path|	2.7|


### Current Performance numbers using Quagga on the S4048 (Intel(R) Atom(TM) CPU S2338 @ 1.74GHz)
#### BGP Link Failover  
| Routes  | Convergence Trial 1 | Convergence Trial 2 | Convergence Trial 3 | Average Convergence |
| :-----: | :-----------------: | :-----------------: | :-----------------: | :-----------------: |
|	5000	|2.1596|	1.7779|	1.94	|1.9592|
|	10000	|3.8947	|3.4441|	3.2116|	3.5168|
|	16000	|5.2454|	5.3546	|4.8973|	5.1658|
 



