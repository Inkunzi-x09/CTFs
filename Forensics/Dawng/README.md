# Dawng Challenge

At the beginning of this challenge, we have to download what seems to be a PNG file, named "wrong.png".

To verify if it is true, we are going to check this information with the followed command-line :

```shell
file wrong.png
	wrong.png: data
```

So, we can see here that is not a real PNG file. Most of the time, as the file is corrupted, you will obtain this answer.

The next step will be to open the file with an hexadecimal editor (here I will use "ghex"), in order to search for some hints that gives information on this type of file.

So, I write :

```shell
ghex wrong.png
```
![image info](../../Images/first.png)

In this result, we can see a lot of bytes with their translations on the right. First idea, we are going to verify the file format (here PNG) by checking the header. The normal header for a PNG file is: `89 50 4E 47 0D 0A 1A 0A`. But here, we have: `89 50 4E 47 CA FE BA BE`. The last 4-bytes seem to be a troll "Cafe Babe". By remplacing them, we obtain the good PNG header.

After that, thanks to previously researchs on PNG file chunks (<a>https://en.wikipedia.org/wiki/Portable_Network_Graphics</a>), there is the IHDR chunk to correct. Here, one more time, we found a troll string "FUCK" traduced in hexadecimal by: `00 00 00 0D 46 55 43 CB`.

We replace this 8-bytes by: `00 00 00 00 49 48 44 52` in order to make appear the "IHDR flag".
