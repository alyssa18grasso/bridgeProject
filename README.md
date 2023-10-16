# APPROACH
We used the approach described by Professor Alan Mislove on Piazza.
Each bridge has an id, a root id, a list of ports, a forwarding table, and a seen messages dictionary.
Each port has an id, a lan, a socket, a last bpdu sent, a list of all bpdus heard, and a state.
A port state is as follows: 0 (root), 1 (designated), 2 (disabled).
In the main loop, we read messages from the socket, seeing them as being recieved from a certain port.
If that message is type bpdu, we add that bpdu to the list of bpdus heard at that port.
Then run a function that changes the root and port tags if needed based on that bpdu that was just added.
If that message is type data, then we add the source to the forwarding table with the port as the key.
We send the message based on the following scenarios. If the the destination is not in the forwarding table,
we broadcast the message to all ports. If the destination is in the forwarding table and the port is not
disabled or the one we recieved the message from, we forward that message to the associated port in the 
forwarding table. If the destination is in the forwarding table but the port is disabled or the port is
the one we recieved the message from, then we do not forward the message.

# CHALLENGES
We were having issues with setting up the spanning tree correctly. Our code did not work with multiple ports
being connected to the same LAN. We went through our code multiple times and could not find the bug, so we did
a manual check in the code while tagging ports. Starting from lower index to higher index, we keep record of
seen LANS. If we have seen a LAN before at a lower index, then we disable the port.
We also ran into challenges with goodput. For intermediate-3.conf, our goodput would fall between the range of
.8% - 1.2%. To increase goodput, we kept track of all messages that had been seen so if they had been seen before,
we knew to discard them. This takes up a lot of memory, but it did increase our effective good output. 

# GOOD FEATURES
A good feature in our code is that it is well organized, and we tried not to keep variables that we didn't need.

# TESTING
We tested using the tests given to us with the config files. If we were failing a test with a config file, we 
would slightly mess with the inputs to see what exact property of that test was the issue. For instance, 
when we were failing simple-7.conf, we found it was because there were multiple ports connected to the same LAN.
For specifically the spanning tree, we drew out the examples for simple-5.conf and simple-6.conf and found what
the port states should be. In the code, we listed the port states in order after they were tagged to check where
the states were wrong. We would use that information to debug. 