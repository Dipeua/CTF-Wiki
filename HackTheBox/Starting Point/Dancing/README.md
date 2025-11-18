# Dancing

![](./Images/banner.png)

What does the 3-letter acronym SMB stand for?

```
Server Message Block
```

![](./Images/nmap.png)

What port does SMB use to operate at?

```
445
```

What is the service name for port 445 that came up in our Nmap scan?

```
microsoft-ds
```

![](./Images/smbclient-enum.png)

What is the 'flag' or 'switch' that we can use with the smbclient utility to 'list' the available shares on Dancing?

```
-L
```

How many shares are there on Dancing?

```
4
```

![](./Images/smbclient-connect.png)

What is the name of the share we are able to access in the end with a blank password?

```
WorkShares
```

What is the command we can use within the SMB shell to download the files we find?

```
get
```