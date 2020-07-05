# Writeup for CTFFriday 2020 July W1 by Muzkkir



In this article, I’m going to explain solutions of NSCTF July Week 1, 2020 CTF challenge theame "Asur" Organized by Net-Square Solutions Pvt. Ltd. and created by Prachi Karad.

This CTF had Five different challenges in which major 4 domain was covered; Stenography, Cryptography, Privilege Escalation, and Web challenges. We have to start from Stenography and Cryptography exploitation to access the system and Privilege Escalation to read the flag file.

<kbd>![alt text](images/1.png)</kbd>


## First Flag

<kbd>![alt text](images/02.png)</kbd>

As the challenge was about the looking for the "MASK" on webpage. I visit source code page and found that.

<kbd>![alt text](images/03.png)</kbd>

<kbd>![alt text](images/04.png)</kbd>

Firstly, I have download all images and try to use Staghide tool to extract data.


<kbd>![alt text](images/2.png)</kbd>

I use Binwalk tool to extract data from the images. I got extracted files from the "mask.jpg"

<kbd>![alt text](images/3.png)</kbd>

```
binwalk -B mask.jpg 
binwalk --dd='.*' mask.jpg
```

<kbd>![alt text](images/4.png)</kbd>

Bingo !!! I got my 1st Flag !!!

<kbd>![alt text](images/05.png)</kbd>

```
NSCTF{Andhakar_Ek_5ach_Hai_Aur_Prakash_3k_Mithya}
```

<kbd>![alt text](images/9.png)</kbd>



## Second Flag

<kbd>![alt text](images/10.png)</kbd>

Now, I need to look for the "nihilist" image.

<kbd>![alt text](images/11.png)</kbd>

Using strings command I extraced words from image "nihilist.jpeg". I used -n query to extract word length more than 9
```
strings nihilist.jpeg -n 9
```
<kbd>![alt text](images/12.png)</kbd>

```
74 47 47 82 73 49 72 64 66 45 83 48 86 47 64 46 64 68 56 57 48 46 84 65 73 75 45 43 55 49 72 47 65
Ask nikhil to open the door
```
I have retrived the 2 important hints. In which first one might be encoding and the second would be the key to extract the flag.

<kbd>![alt text](images/13.png)</kbd>

I visit the "https://cryptii.com/" website to decode the ascii code. Look on the Library !! I found the encoding method.

<kbd>![alt text](images/14.png)</kbd>

> Now Time to Extract the Flag

<kbd>![alt text](images/15.png)</kbd>

```"flag{karunahikrurtahailagaavhipeedahai}"```





## Third Flag


<kbd>![alt text](images/20.png)</kbd>

As the HINT suggest So I started look for "dj" word.
However most of attemps are failed at first.

<kbd>![alt text](images/21.png)</kbd>

<kbd>![alt text](images/22.png)</kbd>

<kbd>![alt text](images/23.png)</kbd>

<kbd>![alt text](images/24.png)</kbd>

<kbd>![alt text](images/25.png)</kbd>

> I looked for every possibilities. :( but did not found anything usefull. Then I got the message !

<kbd>![alt text](images/26.png)</kbd>

I have started looking for "CWE 22" and it is a path traversal vulnerability.

<kbd>![alt text](images/27.png)</kbd>

```So, I went to "http://10.90.137.137/?dj=/etc/passwd"```

<kbd>![alt text](images/28.png)</kbd>

I visit the home folder of Asur and response was hidden.

<kbd>![alt text](images/29.png)</kbd>

But I found on the source page.

```NSCTF{Peeda_5e_Bada_Sh1kshak_Aur_koi_nahi_Hota}```

<kbd>![alt text](images/30.png)</kbd>



## Fourth Flag

<kbd>![alt text](images/41.png)</kbd>

<kbd>![alt text](images/40.png)</kbd>

I quickly visit the Asur's command history file.

<kbd>![alt text](images/42.png)</kbd>

<kbd>![alt text](images/43.png)</kbd>

```asur:b8be16afba8c314ad33d812f22a04991b90e2aaa```
As per my assumptions this must be the SSH login credentials but the password was hash format. For hash value to decode I use online platform named CrackStation where i got the real value of hash file.
```baconandcheese```

<kbd>![alt text](images/44.png)</kbd>

It's time for Login with SSH !! but what !! I face an error !

<kbd>![alt text](images/45.png)</kbd>

Now i visit the "passwd" file in which I tried all other user with same credentials.

<kbd>![alt text](images/47.png)</kbd>

```
22222222222222222222222222222222222222222222222222222222222222222222222222222222
```

<kbd>![alt text](test/26.png)</kbd>

Still, the flag was encrypted !!!

<kbd>![alt text](test/27.png)</kbd>


```
After some random changes in file I found that something was wrong in that line:
"c = (p - (k[i % strlen(k)] ^ t) - i*i) & 0xff;"

So I changed the "-" value to "+" So the code would work perfectly.
c = (p + (k[i % strlen(k)] ^ t) + i*i) & 0xff;
```

<kbd>![alt text](test/28.png)</kbd>


Still somewhere went wrong!!

A few minutes spending on reading the other flags hint !!! and suddenly I remembered the key "use_chandramauli_whenever_you_want" and I changed the decryption key from "whenever_you_want!" to "chandramauli".

<kbd>![alt text](test/29.png)</kbd>


I recompiled and run code & Got Final Flag !!!

```nsctf "!n_m3m0ry_0f_+0ny_5+@rK"```


<kbd>![alt text](test/30.png)</kbd>


## Conclusion

Overall, this CTF was unique for me because I learned about FTP, cryptography, and scripting.  Also, playing this was fun and I appreciate the efforts of the team.


#

**Thank you.**

**Husseni Muzkkir.**
