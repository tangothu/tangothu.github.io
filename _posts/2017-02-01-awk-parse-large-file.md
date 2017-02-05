---
layout: post
title: Use awk to parse plain text large file (from DHCP lease for network IP address lookup) 
categories:
- Programming
tags:
- Shell scripting
- awk
- text parsing

---

We will always need to use low level tools such as shell script to parse log files or plain text files in linux system. In this example we will show how to use awk command and shell script to parse ip leases file (dhcpd.leases) for network router Mac Address match. 

## Use awk to parse plain text large file 

Assume we have plain text file as below:

```
lease 192.168.124.118 {
  starts 1 2014/12/01 12:42:49;
  ends 1 2014/12/01 20:42:49;
  tstp 1 2014/12/01 20:42:49;
  binding state free;
  hardware ethernet 00:0c:29:d5:ff:cb;
  uid "\001\000\014)\325\377\313";
}
lease 192.168.124.117 {
  starts 5 2015/05/01 18:49:10;
  ends 6 2015/05/02 02:49:10;
  tstp 6 2015/05/02 02:49:10;
  binding state free;
  hardware ethernet 00:0c:29:85:6f:62;
  uid "\001\000\014)\205ob";
}
lease 192.168.124.116 {
  starts 5 2015/06/05 14:48:40;
  ends 5 2031/06/05 22:48:40;
  tstp 5 2015/06/05 22:48:40;
  binding state free;
  hardware ethernet 00:0c:29:d1:3e:0d;
  uid "\001\000\014)\321>\015";
}
lease 192.168.123.200 {
  starts 5 2012/07/13 11:54:46;
  ends 5 2031/07/13 11:57:42;
  tstp 5 2012/07/13 11:57:42;
  binding state free;
  hardware ethernet 88:c6:63:c6:08:52;
  uid "\001\210\306c\306\010R";
}
...
...
```

Given a Mac Address (the string after hardware ethernet, e.g. 00:0c:29:d5:ff:cb), our goal is to get the ip address (the string after lease, e.g. 192.168.124.118) with the current time is between the start time (in the first block example, 2014/12/01 12:42:49) and end time (2014/12/01 20:42:49). 

If we use awk, we can come up a command like this:

```
awk 'BEGIN{RS = "}";FS = "\n";}
{
    split($2, start, " ");
    split($3, end, " ");
    if (start[1] == "starts" && end[1] == "ends")
        {
            cmd="date +%Y%m%d%H%M%S";
            cmd | getline currentDateTime;
            startDate=start[3];
            startTime=start[4];
            gsub(/\//,"",startDate);
            gsub(/[:;]/,"",startTime);
            startDateTime=startDate""startTime;
            endDate=end[3];
            endTime=end[4];
            gsub(/\//,"",endDate);
            gsub(/[:;]/,"",endTime);
            endDateTime=endDate""endTime;
            if (currentDateTime >= startDateTime
                && currentDateTime <= endDateTime) {
                for (I=NF;I>0;I--) {
                    split($I, lineArray, " ");
                    second=lineArray[2];
                    if (second == "ethernet") {
                        third=lineArray[3];
                        gsub(/\;/,"",third);
                        split($1, lease, " ");
                        print lease[2], third;
                    }
                }
            }
        }
}' dhcpd.leases
```

Explaination:

awk can be used with the following syntax:

```
awk 'BEGIN{...} {...} END{...}' input-file1
```

BEGIN block is executed once only, before the first input record is read. Likewise, END block is executed once only, after all the input is read.

In our example, we use BEGIN block only. BEGIN block can have multiple input builtin parameters. Because we are using RS and FS, we will explain how these two works. In this example, we choose RS as "}", which means the fields of this file are separated as below, in red blocks. FS is chosen as "\n" (newline), which means for each field it can be further separated by each line as records, in yellow blocks.

![awk RF]({{ site.url }}/images/dhcpd.leases_sample.png)

The variables in each record is stored as a variable starting with "$" sign, so the 

```
lease 192.168.124.118 {
```
is $1, and 

```
starts 1 2014/12/01 12:42:49;
```
is $2, etc. By splitting each the file to fields/records, we are able to add additional logic to the awk code, to see if the current time is between the start and end time of the lease when it's validated.

Finally, key word NF is used to record how many rows are in one field. In this case, for each field we have 8 rows and as such we scan from the end of field to get the Mac Address after key word ethernet. 

The final output after using the awk command to parse dhcpd file is (note the time when parsing this file is in 2017):

```
192.168.124.116 00:0c:29:d1:3e:0d
192.168.123.200 88:c6:63:c6:08:52
```

Thanks for reading!