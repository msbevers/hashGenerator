# hashGenerator
My QA Assignment to test a hash generator binary

provided to JumpCloud as a QA exercise

Author:  Morey Bevers
Phone:   See Resume
EmailL   morey@pobox.com


Initial Thoughts on What to test regarding this binary.

Assuming that I am running this on a MAC OS laptop

(1) Can I invoke the binary?
(2) Does the binary run and stay running?
(3) Report the PID of the running binary
(4) Will it run in the background?
(5) Can I kill it wia a kill -1 PID command
(6) Can I kill it via a kill -9 PID command
(7) Will it run in foreground?
(8) Will a Ctrl-C halt its execution
(9) Can I send a proper Shutdown Curl command and have it stop execution in Foreground process
(10) Can I send a proper Shurdown Curl command and have it stop execution in Background process
(11) Can I start the binary and get stats from it right away? 
(12) Are the initial stats 0?  For Requests?
(13) Are the initial stats 0? For Average time?  In Milliseconds?
(14) If the Port is not set appropriately, what is the failure mechanism?  Error Messages?   What happens?
(15) What if the Port is not set at all?  What is the failure mechanism?  Error Messages?  What happens
