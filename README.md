TryHackMe Pickle Rick Write-Up
Today, we will be tackling the Pickle Rick challenge from TryHackMe!
You can find it here: TryHackMe - Pickle Rick
Disclaimer - Your IP Address Will Differ from Mine!
Scanning & Enumeration
A great starting point is to scan your machine for any open ports.
Command: nmap -T4 -sC -sV -A -p- 10.10.82.95
Command Breakdown:
-T4: Sets the speed of the Nmap scan to 4 out of 5 (a personal preference).
-sC: Scans using default NSE scripts, which are useful for discovery and considered safe.
-sV: Attempts to determine the version of the service running on each port.
-A: Performs an extensive scan on the identified ports.
-p-: Scans all ports.







Enumerating Services
Port 22 - SSH
We can see that the SSH port is open. This port is generally secure unless we have access to someone's credentials, so we won't interact with it further.
Port 80 - HTTP
Here, we discover a web service. Let's explore to see if there's anything interesting.






It appears we have a webpage, and Rick seems to need our assistance. Checking the page's source code can reveal valuable information.






Great! We've found a username. Let's take note of it and continue our search for the password.
Finding Hidden Directories
We'll use Gobuster to locate any hidden directories.
Command: gobuster -u http://10.10.82.95 -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -x php,sh,txt,cgi,html,css,js,py
Command Breakdown:
-u: Full URL (including scheme) or base domain name.
-w: Path to the wordlist used for brute-forcing (use – for stdin).
-x: List of extensions to check for.









It looks like GoBuster has uncovered a few directories. Let's check out login.php.






Earlier, we found a username, but we still lack a password. Most websites have a robots.txt file that indicates what is and isn't allowed to be indexed. Let's see if this site has one.
Success! There is a robots.txt file, and it contains a clear reference to the show Rick and Morty. Let's try this as our password.






YES! We successfully logged in and were directed to the command portal.






Since this page is called the command portal, let's try entering some commands to see what happens. When we enter the ls command, we receive some results, and it seems we've found our first ingredient.
Unable to render image





I wonder if we can access Sup3rS3cretPickl3Ingred.txt in the same way we accessed the robots.txt file. I navigated to http://<ipAddress>/Sup3rS3cretPickl3Ingred.txt, and to my surprise, we now have the content of that text file. One down, two to go!






Digging Deeper
Now that we have one ingredient, it's time to find the other two. Where should we start looking? Let's return to the command portal and use the ls command again. We see a file named clue.txt; 






The phrase "file system" stands out to me. Let's go to the command portal and use ls /home.
This will list everything in our home directory, and look! There's a directory called rick. Let's dig deeper by using ls /home/rick, and within the rick directory, we find a file named second ingredients.






I wonder if we can use the cat command to retrieve the content of the second ingredient. Let's give it a try.
Unfortunately, we encounter an error indicating that the command has been disabled.





Research
find an alternative command to cat.
less command
Command: less '/home/rick/second ingredients'






Well, hot ham! It worked, and we now have the second ingredient.
Now that we know the less command is effective, we need to track down the third and final ingredient. Let's see if we can use ls to access the /root directory. It seems we can't view anything from /root. I wonder if we can use sudo to elevate our user privileges.
Command: sudo -l






WHAT?! We can combine sudo with any command without being prompted for a password. This might be just what we need to see what's in the /root directory. We just need to combine sudo with ls to check the contents of that directory.






Now that we've located the third ingredient, we'll use our friend less to retrieve its contents. Remember, since we're in the root directory, we need to prepend sudo to our command.
Command: sudo less /root/3rd.txt
Ingredient 3](images/ing3.png "Ingredient 3")

YAyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy

DON!!DONE!!!!DONE!!!!!!!
