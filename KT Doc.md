

- How does the OAuth work
	- 
- How to run tinysix
	- Command:`sudo rlwrap ./edge.native  -d /dev/ttyUSB0 -b 460800 -a fd00::1 -i tinysix0 -c 0 -p 0xfaad -v  RPL=INFO EDGE=INFO -m 11`
	- Explanation: `rlwrap` is linux utility used to run the `edge.native` binary (which is tinysix program)
	- CHECKLIST: 
		- [ ] ensure that channel and panid are same as that for the NIC module in meter
		- [ ] ensure that the ROOT nic card has compatible firmware with the meter NIC 
		- [ ] ensure that the device `/dev/ttyUSB0` or `/dev/ttyACM0` or equivalent is checked. 
			- [ ] to check which ports are available you can use `ls /dev/tty*` and check for relevant port
	- Connecting with meters
		- `ip-routes`
		- `ip-ping fdaa:ipv6::of::meter::fdaa` = used to ping the link local address of the meter. as of dec 2023, you can ping either of the address for the meter - the one starting with `fd00` or the one starting with `fe80`. 
			- alternatively, you can ping the meter via linux command line utlity called `ping fdaa:ipv6::of::meter::fdaa`. Take note that this can be only used with `fd00` (and does't work with `fd00`)
		- `log all 3 ` = set logging level  to 3 (warn)
	- Debugging the output
		- there should be at least one `DIO` cast send to ensure that the Root NIC module (connected to dev laptop and acting as DCU) has initialised properly. 
		- In some cases, if you can't detect the meters, try to relaunch the `rlwrap` command or try to disconnect the launchpad physically and re start from step 1.
- How to run tnest-edge
	- Basics 
		- The application is dockerized. docker must be installed. prior to running the app, the environment variables should be set in `.envrc`and loaded in shell. `tnest-edge` will pick up relevant environment variables accordingly. make sure you export the variables like : `export ENV_VARIABLE=vale` 
		- any command you want to run, should be prefixed with ` ./scripts/run-dockerized.sh ` examples: 
			- ` ./scripts/run-dockerized.sh mix deps.get`
			- ` ./scripts/run-dockerized.sh mix ecto.migrate`
			- ` ./scripts/run-dockerized.sh iex -S mix phx.server`
			- the reason is that we need to invoke the mix and elixir installed inside docker, not the one on our local machine.
			- if you get errors in Tnest.Command.cast , you need to stop the server and manually run ` ./scripts/run-dockerized.sh mix compile.protocols` and re run the server


- How to setup a meter in tnest-edge
	- Two ways: 
		1. `Add new meter` on left sidebar on meter table page on route: `/metering`
			1. create a new meter using the multi step meter form. insert the correct 
		2. `Setup Guide ` under `Actions` on meter overview page. on route: `/metering/:resource_id_of_meter`
			1. A modal will open up which only updates the meter since the meter is already added (if meter overview page is visible , it means that meter exists in db)
- How does the transactions/workflows work
	- What is a workflow ? 
		- Concurrency limits and further details 
	- What is a transaction? 
		- What is
	- What is a mutex?
- How to run centient 
	- runs like any elixir application. `mix deps.get ; mix ecto.crate ; mix ecto.migrate ; iex -S mix phx.server `
- How to get tnest-edge <> centient to exchange data and be able to do requests
	- Start vernemq :  `sudo docker run -p 1883:1883 --name vernemq --restart=always -e DOCKER_VERNEMQ_ALLOW_ANONYMOUS=on -e DOCKER_VERNEMQ_ACCEPT_EULA=yes vernemq/vernemq` : this will ensure that an MQTT broker can relay messages from tnest-edge to centient
	- define environment variables in `.envrc `of tnest-edge 
		- `export REMOTE_MQTT_HOST=localhost 
		  `export REMOTE_MQTT_PORT=1883`
		  `export REMOTE_SYNC_ADAPTER=Tnest.Integration.Centient.MQTT`
- Briefly, your understanding of all the metering configuration pages in tnest-edge/centietn
- passwords 
	- Laptop : 3Tinymesh! (both disk and login)
	- Meghraj : tinymesh123