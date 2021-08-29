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
