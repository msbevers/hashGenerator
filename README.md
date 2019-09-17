# hashGenerator
My QA Assignment to test a hash generator binary

provided to JumpCloud as a QA exercise

Author:  Morey Bevers
Phone:   See Resume
EmailL   morey@pobox.com


Initial Thoughts on What to test regarding this binary.

Assuming that I am running this on a MAC OS laptop


Manual Tests:

(1) Can I invoke the binary?
(2) Does the binary run and stay running?
(3) Report the PID of the running binary
(4) Will it run in the background?
(5) Can I kill it via a kill -1 PID command
(6) Can I kill it via a kill -9 PID command
(7) Will it run in foreground?
(8) Will a Ctrl-C halt its execution
(9) Can I send a proper Shutdown Curl command and have it stop execution in Foreground process
(10) Can I send a proper Shutdown Curl command and have it stop execution in Background process
(11) Can I start the binary and get stats from it right away? 
(12) Are the initial stats 0?  For Requests?
(13) Are the initial stats 0? For Average time?  In Milliseconds?
(14) If the Port is not set appropriately, what is the failure mechanism?  Error Messages?   What happens?
(15) What if the Port is not set at all?  What is the failure mechanism?  Error Messages?  What happens?
(16) Run the POST command to hash using the password "angrymonkey"  (Identifier returned and then 5 seconds of total time returned? Contents of file 42 contains the request number?  1st time should be 1, 2nd time 2 etc.
(17) Run the POST command again with the same password "angrymonkey"  (Identifier returned immediately followed by 5 seconds?)  Contents of file 42?    2nd time it should be 2.
(18) Try password "", does it get encoded?  Error Message?  Crash?
(19) Try password with 32 characters, 64, characters, 128 characters, 256 characters.  What is the limit?  What does the development team say?  How does it fail?  gracefully?
(20) Come up with a way to make sure SHA512 is being used.  Is there a way to verify this? Are 64 characters (bytes) returned?  SHA512 should return 64 bytes.  Verify.
(21) With a POST of 2 passwords that are identical, AngryMonkey, can we verify that the same hash is returned?  Use the GET command for /hash/1 and /hash/2.  Should return same hash
(22) With a GET command and invalid Identifier, what happens?  Graceful?  Meaningful Message?  Get Stats remain the same?
(23) With a GET command and no identifier, what happens? Graceful?  Meaningful Message? Get Stats remain the same?
(24) Determine how large the hash table can be. What is supported by Developement.  Try to add one more than supported.
(25) Does a GET to /hash return a BASE64 encoded hash?  Is there a way to verify this is BASE64?
(26) Try a GET to /stats/1 (appending data) What happens?  Graceful?   Meaningful Message?
(27) Does a Shutdown and re-invocation of the binary start the number of requests over at 0 again?  Use /stats to find out
(28) Verify that the average-times are greated than 5 seconds, since we wait for 5 seonds before generating the hash.  Is this the desired outcome?

Bugs Identified:
Average time < 5 seconds.  Should be more than 5 seconds?  per spec?  5 sedonds then hash generation?


