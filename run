#!/bin/bash

# Check to see if service is running by pinging a specific port every hour.
# Service / process is assumed dead if its port is closed.
# If dead, alert email is sent to specified addresses.
# @juanvallejo

FALSE=1
TRUE=0

is_service_alive() {
	
	if [[ -z "$1" ]]
	then
		echo "Warning: no service port specified"
		return $FALSE
	fi
	
	unset IS_PORT_OPEN
	IS_PORT_OPEN=`nmap -PN -p $1 localhost | grep open`

	if [[ -z "$IS_PORT_OPEN" ]]
	then
		return $FALSE
	else
		return $TRUE
	fi

	return $FALSE

}

PORT="$1"
MESSAGE="Hi there,\n\nI am a small program written to keep track of the Pizza My Mind server status.\n\nThis message is to let you know that the Pizza My Mind API server has recently experienced down time. I have attempted to bring the server back up, but please double check this by visiting http://fenrir.pcs.cnu.edu.\n\nTo restart the server manually, try the command below on a unix terminal (if it asks for a password, type \"pcse\"):\n\nssh juan@fenrir.pcs.cnu.edu \"echo pcse | sudo -S service pmm-server start\"\n\nA recent copy of the logs from the server is attached below. This might help you debug any issues that might have caused the server to crash. A copy of the entire log can be found in /home/juan/pcse-scanner-api-runtime/log.txt.\n\n"
MESSAGE=$MESSAGE`tail /home/juan/pcse-scanner-api-runtime/log.txt`

if [[ -z "$PORT" ]]
then
	PORT="7777"
fi

N_ALERTS_SENT=0

while true
do
	if is_service_alive "$PORT"
	then
		N_ALERTS_SENT=0
		echo "Port $PORT is open. Assuming nominal state"
	else
		echo "Port $PORT is closed. Server assumed dead"
		
		# generate email
			
		if [[ "$N_ALERTS_SENT" -le 3 ]]
		then
			if [[ "$N_ALERTS_SENT" -eq 0 ]]
			then	
				echo "Alert email sent."
				echo -e $MESSAGE | mail -s "The Pizza My Mind Server is DOWN" -r "juan.vallejo.12@cnu.edu" --to "juan.vallejo.12@cnu.edu" "clarem@cnu.edu" "jonah.lazar.14@cnu.edu" "raymond.koehl@cnu.edu"
				# attempt to bring server back up
				echo "Attempting to restart server..."
				sudo service pmm-server start
			fi
			N_ALERTS_SENT=$[$N_ALERTS_SENT + 1]
		else
			N_ALERTS_SENT=0
			echo "Resetting email timeout count."
		fi	
	fi
	sleep 1h
done
