# PClub-DVWA-Task
Now firstly we are given a site which has a blogs and a gallery section and links to the og website and social media.

![image](https://github.com/Sohamp05/PClub-DVWA-Task/assets/142091197/09eea5fc-e3ab-47b8-9cb4-0f467c1d91d9)

## The First Flag
Since there is nothing much in the home page's source code so when we move to gallery section's source code we find the images are brought using getFile api and in the description we are given that a routes.txt file is there for all the routes and it is in the website's directory only:

![image](https://github.com/Sohamp05/PClub-DVWA-Task/assets/142091197/0e0f8281-0bb5-404f-86d6-84583bed8174)

So we put the following in the URL:
>http://74.225.254.195:2324//getFile?file=/home/kaptaan/PClub-DVWA/routes.txt

and BAMM!!

![image](https://github.com/Sohamp05/PClub-DVWA-Task/assets/142091197/c701e2bc-7994-434e-8952-e12fbf2a21bc)

we got the contents of the txt file and the first flag!

`pclub{path_trav3rsa1_1s_fun}`

## The Second Hunt
Now we can see different webpages that may exist or routes which may exist in the website and hint to the way to our second treasure!

We need to get MAC address of eth0 interface of the device on which this website is hosted and command injection could come to help!
After exploring 2-3 paths and researching about command injection, you can find the IP Address page, which looks similar to DVWA command injection exploits:
`http://74.225.254.195:2324/ipDetails`
We discover we can run commands on terminal after putting a semicolon:
for example:

![image](https://github.com/Sohamp05/PClub-DVWA-Task/assets/142091197/c62bd9c7-dc31-475c-a935-f45997e4784f)

now to find MAC address we put in the command
`; ifconfig eth0`

![image](https://github.com/Sohamp05/PClub-DVWA-Task/assets/142091197/b95a2e93-0b6f-449a-afc8-b325cbd114e1)

We find the MAC address : 60:45:bd:af:3f:e5
and thus the flag being

`pclub{60:45:bd:af:3f:e5}`

## Third Flag (?)
Ok now for a writeup this could be a little controversial but but so, this is how i proceeded further:
I saw the login page, tried a lot of SQLi commands but to fail and even tried figuring ways to break through burpsuite but to fail(maybe i need to polish those skills!)
I noticed cat, ls and other commands are being filtered out and not being passed so, i searched to bypass those filters maybe and got a great page:

`https://0x80dotblog.wordpress.com/2021/07/28/os-command-injection-tutorial-part-1-basics-and-filter-evasion/`

and tried a few commands and i was able to break in and got access to the whole website's files:

![image](https://github.com/Sohamp05/PClub-DVWA-Task/assets/142091197/25d9286c-a304-4071-b644-b2ef61b91f63)

and after searching the files, i got the database to the logins and by loggin in to kaptaan's account which i found in create_data.py, i got the flag's file and the flag
`pclub{01d_1s_g01d_sql1}`
and by logging in to Ritvik's account I got the link to crypto/pwn challenge
`https://pastebin.com/Jd17mvk5`
and got an image from aman's account:

![ariitk](https://github.com/Sohamp05/PClub-DVWA-Task/assets/142091197/a560b894-90e9-436b-addc-aeae8bf45f8a)

## Fourth Decryption
Seeing an image of course it is steganography!, first lazily i tried stegonline.georgeom.net but got nothing and thus i opened the terminal and ran

![photo_6233210888098266415](https://github.com/Sohamp05/PClub-DVWA-Task/assets/142091197/1b78c3a4-1ab6-4ecb-b45b-a9ec7f609bca)

and extracting the file we get a qr image

![image](https://github.com/Sohamp05/PClub-DVWA-Task/assets/142091197/3dc33bc8-f7f6-44a8-8f23-66a2baa6cb0e)

and scanning it we get b64 encoded flag and then a caesar cipher encrypted also:

`pclub{data_1s_3v3ryth1ng}`

## Fifth remaining destination
Now we download the file and rev it using ghidra and we get the following main,check,shachecker functions:

### main
![photo_6233210888098266459_y](https://github.com/Sohamp05/PClub-DVWA-Task/assets/142091197/adca00ba-c9a5-420a-abb7-2997af374386)

## check
![photo_6233210888098266458_y](https://github.com/Sohamp05/PClub-DVWA-Task/assets/142091197/752722b0-6df2-4515-9205-273cfb2ec1f5)

## shachecker
![photo_6233210888098266459_y](https://github.com/Sohamp05/PClub-DVWA-Task/assets/142091197/2726cb6b-b1cf-4543-9384-99e9ec321930)

Now we can easily see that if we find the sha1 decryption of `c832724069b0cf39e47a041d11ef9ebb9c718c0e` then that password could be used to obtain the flag netcating,
but I wasn't able to the decryption after using 3 different rockyou files and hashcat and also, there was a video of a chinese song's DJ remix at the end but I wasn't able to find what it was suggesting to...

## Third Again
Now, since seeing the flag maybe bypassing filter wasn't the right way as it is of SQLi so, I further explored the areas which I thought were irrelevant, like the Blogs section and I found it was getting the flag from a SQL database apparently, so, we had to use SQL injection in that and seeing the scripts using bypassing I saw that the blogs were indeed taken from the same database as username, password and we get the password as md5 hash so, it could be decrypted and found passwords of the said usernames.

## Conclusion
So, now 3 flags I guess I have found using the ways they were to be found, and the 3rd flag through which I have gotten the files for 4th and 5th flag, I took a different route and got all server files and using that found the 4th and files for 5th.
