# Python Covert Channel
Designed a local covert channel between two processes.

## Implementation:
My covert channel is set up to be a one-way communication but could easily be revised to
make it so the two processes can communicate back and forth. Thus, in my implementation there is
a sender and receiver process. Both processes can write to their respective files but cannot read or
write to any file that is not their own. The way they communicate is by detecting the changes in file
size of the other process’ file.

Both processes execute trivial commands such as ping and arp. They use a pipe to redirect
these command outputs to their respective files. As they execute these commands, the files increase
in byte size. If P1, the sender process, wanted to relay a message to P2, the receiver process, it will
run the ping command and redirect the output to its respective text file. It will continue to do this
and add small buffer characters until the byte size of the file indicates an ascii value plus a
predetermined offset. P2 will watch the ping file until the file size stops changing. It will use that
information to record a character value.

The covert channel communicates in a three-way handshake. Upon detecting and translating
the covert data, P2 will apply changes to its file by redirecting the output of the arp command. P1
will see that the arp file byte size has changed and use this as an indication that the message is
received. Next, P1 will reset the ping file to a byte size lower than the offset value. P2 will take this
change in byte size as an indication the P1 has received confirmation that P2 got the package and is
about to send another character. This sequence of transactions continue until a null character is
relayed, indicating to P2 that no more characters will be sent. It is also important to know that while
P1 and P2 are using ping and arp output to fill their files, it doesn’t matter what is put into the file,
but in this example, these are the commands used to add content to the file. 

*Full Report*: [Click Here](https://github.com/Reaganak40/process_cover_channel/blob/main/Cpts%20427%20HW3%20Covert%20Channel.pdf)

*Demo Video*: [Click Here](https://wsu.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=ea7bff91-dfe0-4e4f-87bb-ae8701748ce0&start=0)