# 2_Phase_Commit
2 Phase Commit Protocol Simulation

This is a Java Swing and MySQL based project. The server communicates with its clients using HTTP protocol and sockets. 
Server: Server just acts as a medium for transferring messages from sender and receiver. All the received GET and POST requests are displayed on the server’s user interface. A sender process sends a message in a HTTP POST request and a receiver process polls the server to receive its messages using HTTP GET request. The server temporarily stores the message in a database table until they are delivered to the receiver as a response to its GET request.
Coordinator: The coordinator process sends an arbitrary string to all participant process which the participants will write to their log file after a distributed commit. After sending the string, the coordinator will send a vote request message to every participant and waits for the reply. After receiving reply from all participants, it sends a GLOBAL_COMMIT or GLOBAL_ABORT message to commit or abort the transaction. If reply is not received from all participants within 30 seconds, the coordinator will send a GLOBAL_ABORT to every participant.
Participants: Run 3 instances of the HTTPMultithreadedSocketClient class which will act as the participant processes. These processes will receive a string from coordinator and wait for a vote request. If vote request is not received within 30 seconds, the participant will write VOTE_ABORT in their log file. If a vote request is received from the coordinator, then the buttons to vote for commit or abort will be enabled. After user clicks on a button, the vote is sent to the coordinator and participant waits for a decision from the coordinator. After receiving the decision, it either commits the transaction (writing string to the file) or aborts it. If decision is not received within 30 seconds, then the participant sends a DECISION_REQUEST message to other participants and waits until a decision to commit or abort the transaction is received.

To run the project:
1)	Create a table named ‘messages’ in your MySQL database using the file ‘http_socket_db.sql’ provided in the zip.
2)	Update the connection string in connectToMySql() method of ‘HTTPMultithreadedSocketServer.java’.
3)	Include the library ‘mysql-connector-java-5.1.45-bin.jar’ in your classpath. It is a JDBC library for MySQL connectivity.
4)	Start the server process by running ‘HTTPMultithreadedSocketServer.java’.
5)	Start the coordinator process by running ‘HTTPMultithreadedSocketCoordinator.java’.
6)	Start 3 client processes by running ‘HTTPMultithreadedSocketClient.java’.
7)	Click on the ‘Start Transaction’ button on coordinator user interface to initiate a transaction.
8)	Click on either ‘Commit’ or ‘Abort’ button on participant user interfaces.
9)	The transaction will commit or abort as per the 2-phase commit protocol.
10)	To disconnect the client, send a message as ‘exit’.
