

- How does the OAuth work
	- 
- How to run tinysix
	- `sudo rlwrap ./edge.native  -d /dev/ttyUSB0 -b 460800 -a fd00::1 -i tinysix0 -c 0 -p 0xfaad -v  RPL=INFO EDGE=INFO -m 11`
		- `rlwrap` is linux utility used to run the `edge.native` binary (which is tinysix program)
		- CHECKLIST: 
			- [ ] ensure that channel and panid are same as that for the NIC module in meter
			- [ ] ensure that the ROOT nic card has compatible firmware with the meter NIC 
			- [ ] ensure that the device `/dev/ttyUSB0` or `/dev/ttyACM0` or equivalent is checked. 
				- [ ] to check which ports are available you can use `ls /dev/tty*` and check for relevant port
- How to run tnest-edge
	- Basics 
		- The application is dockerized. docker must be installed. prior to running the app, the environment variables should be set in `.envrc`and loaded in shell. `tnest-edge` will pick up relevant environment variables accordingly. make sure you export the variables like : `export ENV_VARIABLE=vale` 
		- any command you want to run, should be prefixed with ` ./scripts/run-dockerized.sh ` examples: 
			- ` ./scripts/run-dockerized.sh mix deps.get`
			- ` ./scripts/run-dockerized.sh mix ecto.migrate`
			- ` ./scripts/run-dockerized.sh iex -S mix phx.server`
			- the reason is that we need to invoke the mix and elixir installed inside docker, not the one on our local machine.
	- Connecting with meters
	- Debugging the output

- How to setup a meter in tnest-edge
	- Two ways: 
		1. `Add new meter` on left sidebar on meter table page
			1. create a new meter using the multi step meter form. insert the correct 
		2. `Setup Guide ` under `Actions` on meter overview page. 
			1. this only updates the meter since the meter is already added (if meter overview page is visible , it means that meter exists in db)
- How does the transacations/workflows work
	- What is a workflow ? 
		- Concurrency 
	- What is a transaction? 
		- What is
	- What is a mutex?
- How to run centient (docker commands and all)
- How to get tnest-edge <> centient to exchange data and be able to do requests
- Briefly, your understanding of all the metering configuration pages in tnest-edge/centietn
- passwords 
	- Laptop : 3Tinymesh! (both disk and login)
	- Meghraj : 