# hashGenerator
My QA Assignment to test a hash generator binary

provided to JumpCloud as a QA exercise

Author:  Morey Bevers
Phone:   See Resume
EmailL   morey@pobox.com


Below is a list of Manual Tests that I was able to come up with initially.  With more time, I would flesh these out more.
I have written up a Test Case (Test Procedure) for number 16 below as a representative example of what I think is needed in a test procedure that has been run..   I assumed that you are looking for formatand functionality and that you were not so interested in my writing up test procedures for all of the below on the list.

In create test casesm I normally would include example screenshots, example screen output, any test scripts, or commands needed in order to make the test procedure easier to run.   In the event that the binary is not yet avaialble, I would work with development or make some assumptions and point the test engineer in the right direction.

A completed test procedure (executed) would normally include screenshots of actual results where applicable, screen output, test log or trace output,  etc with my recorded test results and a clear indication of PASS vs. FAIL and in the event of a failure, the BugID associated with the failure.

The same goes for a bug report.  I have included a representative eug report in the repo as well.  I normally would add screenshots, test logs, screen output, test scripts etc with my recorded test results. 
In addition, anything that JumpCloud requires, I would include as well.  I am sure you are using Jira and Testrails, which I have not used, but it cannot be much different that the test tools that Oracle is presenlty using.   The tools are secondary to how one utilizes them.

Initial Thoughts on What to test regarding this binary. (both positive and negative tests)


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
(16) Run the POST command to hash using the password "angrymonkey"  (Request Identifier returned Immediately and then 5 seconds delay then Hash entry added?  1st time should be 1, 2nd time 2 etc.  Stats updated by 1?  Time changed?
(17) Run the POST command again with the same password "angrymonkey"  (Request Identifier returned immediately and then followed by 5 seconds?)  Hash entry added?    2nd time it should be 2. Staus updated by 1?  Time changed?
(18) Try password "", does it get encoded?  Error Message?  Crash?
(19) Try password with 32 characters, 64, characters, 128 characters, 256 characters.  What is the limit?  What does the development team say?  How does it fail?  gracefully?
(20) Come up with a way to make sure SHA512 is being used.  Is there a way to verify this? Are 64 characters (bytes) returned?  SHA512 should return 64 bytes.  Verify with https://emn178.github.io/online-tools/sha512.html?  (Possible bug?  34dd0f00ab6279aca24d8f3f41de7701e3331e46ef6437706188839f0b4376ffc5216bdccb5b0a09beea8bb36ef10f0277f32a8d07b2088d2958a0c6a7be00d6 is the answer returned which differs
Verifying with a second source for SHA512 hash.  It also differs.  See https://passwordsgenerator.net/sha512-hash-generator/.  Note these 2 sources agree but our hash generator binary does not.  Needs a bug.

(21) With a POST of 2 passwords that are identical, AngryMonkey, can we verify that the same hash is returned?  Use the GET command for /hash/1 and /hash/2.  Should return same hash
(22) With a GET command and invalid Identifier, what happens?  Graceful?  Meaningful Message?  Get Stats remain the same?
(23) With a GET command and no identifier, what happens? Graceful?  Meaningful Message? Get Stats remain the same?
(24) Determine how large the hash table can be. What is supported by Developement.  Try to add one more than supported.
(25) Does a GET to /hash return a BASE64 encoded hash?  Is there a way to verify this is BASE64?
(26) Try a GET to /stats/1 (appending data) What happens?  Graceful?   Meaningful Message?
(27) Does a Shutdown and re-invocation of the binary start the number of requests over at 0 again?  Use /stats to find out
(28) Verify that the average-times are greated than 5 seconds, since we wait for 5 seonds before generating the hash.  Is this the desired outcome?
(30) Verify that multiple connections can be supported simultaneously:   bash script while true do curl POST a new entry done;  Run this in multiple Terminal sessions for a while (10 minutes?  1 hour?) 
(31) Verify that a shutdown will complete any in-flight requests and will not process those received later.  Check the last hash entry for completeness
(32) Verify that good error messages are passed for #31 above when the shutdown is processedm and the the hash table is complete.
(33) Repeat much of the above with the -i switch in the curl command.
(34) Repeat much of the above with the -v switch in the curl command.
(35) Redirect the output to a file to get the curl timing chart  
     ex:   curl  --noproxy '*'  -X POST -H "application/json" -d '{"password":"sobermonkey"}' http://127.0.0.1:8088/hash > output
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100    28    0     2  100    26      0      5  0:00:05  0:00:05 --:--:--     0
Moreys-MacBook-Pro:hashGenerator mbevers$ 




Examples of execution I ran yesterday:


Initial Conditions:

Moreys-MacBook-Pro:hashGenerator mbevers$ curl -H -XGET http://127.0.0.1:8088/stats; echo ""
{"TotalRequests":0,"AverageTime":0}

================

POST /hash

curl  --noproxy '*'  -X POST -H "application/json" -d '{"password":"angrymonkey"}' http://127.0.0.1:8088/hash  

Moreys-MacBook-Pro:hashGenerator mbevers$ curl  --noproxy '*'  -X POST -H "application/json" -d '{"password":"angrymonkey"}' http://127.0.0.1:8088/hash
1Moreys-MacBook-Pro:hashGenerator mbevers$ 

Note that the Request Id of 1 was returned above after the 5 seconds.  See BugId 101 below


================


GET stats

Moreys-MacBook-Pro:hashGenerator mbevers$ curl -H -XGET http://127.0.0.1:8088/stats; echo ""
{"TotalRequests":1,"AverageTime":300657}

Note that the TotalRequests incremented by 1, but should not the average time be 5.300657 seconds or 5300657 milliseconds?  See BugID 102 below



================


GET hash/1

Moreys-MacBook-Pro:hashGenerator mbevers$  curl -H -XGET http://127.0.0.1:8088/hash/1; echo
NN0PAKtieayiTY8/Qd53AeMzHkbvZDdwYYiDnwtDdv/FIWvcy1sKCb7qi7Nu8Q8Cd/MqjQeyCI0pWKDGp74A1g==

Repeatable?
Moreys-MacBook-Pro:hashGenerator mbevers$  curl -H -XGET http://127.0.0.1:8088/hash/1; echo
NN0PAKtieayiTY8/Qd53AeMzHkbvZDdwYYiDnwtDdv/FIWvcy1sKCb7qi7Nu8Q8Cd/MqjQeyCI0pWKDGp74A1g==




================



Add four more hash entries:

Moreys-MacBook-Pro:hashGenerator mbevers$ curl  --noproxy '*'  -X POST -H "application/json" -d '{"password":"angrymonkey"}' http://127.0.0.1:8088/hash
2

Moreys-MacBook-Pro:hashGenerator mbevers$ curl  --noproxy '*'  -X POST -H "application/json" -d '{"password":"angrymonkey"}' http://127.0.0.1:8088/hash
3

Moreys-MacBook-Pro:hashGenerator mbevers$ curl  --noproxy '*'  -X POST -H "application/json" -d '{"password":"angrymonkey"}' http://127.0.0.1:8088/hash
4
Moreys-MacBook-Pro:hashGenerator mbevers$ curl  --noproxy '*'  -X POST -H "application/json" -d '{"password":"happymonkey"}' http://127.0.0.1:8088/hash
5
Moreys-MacBook-Pro:hashGenerator mbevers$ 


================


GET the HASH for the five new entries:

Moreys-MacBook-Pro:hashGenerator mbevers$ curl -H -XGET http://127.0.0.1:8088/hash/1; echo ""
NN0PAKtieayiTY8/Qd53AeMzHkbvZDdwYYiDnwtDdv/FIWvcy1sKCb7qi7Nu8Q8Cd/MqjQeyCI0pWKDGp74A1g==

Moreys-MacBook-Pro:hashGenerator mbevers$ curl -H -XGET http://127.0.0.1:8088/hash/2; echo ""
NN0PAKtieayiTY8/Qd53AeMzHkbvZDdwYYiDnwtDdv/FIWvcy1sKCb7qi7Nu8Q8Cd/MqjQeyCI0pWKDGp74A1g==

Moreys-MacBook-Pro:hashGenerator mbevers$ curl -H -XGET http://127.0.0.1:8088/hash/3; echo ""
NN0PAKtieayiTY8/Qd53AeMzHkbvZDdwYYiDnwtDdv/FIWvcy1sKCb7qi7Nu8Q8Cd/MqjQeyCI0pWKDGp74A1g==

Moreys-MacBook-Pro:hashGenerator mbevers$ curl -H -XGET http://127.0.0.1:8088/hash/4; echo ""
NN0PAKtieayiTY8/Qd53AeMzHkbvZDdwYYiDnwtDdv/FIWvcy1sKCb7qi7Nu8Q8Cd/MqjQeyCI0pWKDGp74A1g==

Moreys-MacBook-Pro:hashGenerator mbevers$ curl -H -XGET http://127.0.0.1:8088/hash/5; echo ""
S5/PhOdFlRaX9BbgSntGQgpW2aoJVzTou3C/zbUTQ3BqfWlimJf6aTqLfOvxdEAfQHlUiI1i3QodfZRl/k11AQ==

Note that the first 4 entries return the same hash.  They should.  Hash 5 is unique and should return a different hash.



================




Are the stats updated to reflect 5 entries?


curl -H -XGET http://127.0.0.1:8088/stats; echo ""
{"TotalRequests":5,"AverageTime":136078}

Yup.


================

Demonstrate Shutdown:

While running:
Moreys-MacBook-Pro:hashGenerator mbevers$ ps -ef | grep darwin
  501 53806 88661   0  3:33PM ttys000    0:00.03 ./broken-hashserve_darwin
  501 54645 51247   0  4:42PM ttys001    0:00.00 grep darwin


Moreys-MacBook-Pro:hashGenerator mbevers$ curl --noproxy '*' -X POST -H "application/json" -d 'shutdown' http://127.0.0.1:8088/hash; echo ""


after execution:

Moreys-MacBook-Pro:hashGenerator mbevers$  ps -ef | grep darwin
  501 54731 51247   0  4:50PM ttys001    0:00.00 grep darwin
Moreys-MacBook-Pro:hashGenerator mbevers$ 




================

Bugs Identified during the execution:  (Note each of these would be detailed in a Bug Report and the ID added to the Test Procedure executed that exhibited the issue.


(101) The Identifier is returned after the 5 seconds, not immediately per the spec and before the 5 second delay
(102) Average time Reported by Stats is less than 5 seconds.  Should it be more than 5 seconds?  per spec?  5 seconds then hash generation?  Would need to consult with Dev on this one. May not be a bug.
(103) stdout message upon shutdown misspelled. 
  ex:
  2019/09/17 16:50:19 Shutdown signal recieved  ("should be received")
  2019/09/17 16:50:19 Shutting down
(104) SHA512 result not correct?  34dd0f00ab6279aca24d8f3f41de7701e3331e46ef6437706188839f0b4376ffc5216bdccb5b0a09beea8bb36ef10f0277f32a8d07b2088d2958a0c6a7be00d6 returned via alternate source.
  vs NN0PAKtieayiTY8/Qd53AeMzHkbvZDdwYYiDnwtDdv/FIWvcy1sKCb7qi7Nu8Q8Cd/MqjQeyCI0pWKDGp74A1g== by binary.  A secondary site returns 34DD0F00AB6279ACA24D8F3F41DE7701E3331E46EF6437706188839F0B4376FFC5216BDCCB5B0A09BEEA8BB36EF10F0277F32A8D07B2088D2958A0C6A7BE00D6, same as the first site, but different than the binary.  See https://passwordsgenerator.net/sha512-hash-generator/   Pretty sure there is a bug in the generation of our SHA512 Hash
(105) No usage info when invoking binary with -v, -h, -?.
(106) No version id for binary supported reported by the binary itself.


I have documented BugReport_101 above in a separate file to be added to the GIT repo.  This is a report format I adopted from the web for this exercise.  Leaves a few things to be desired, but it gets the point across.
I have documented TC-016 from the list above as a documented test case.   I am not too happy with format I adopted from the web, but I am sure Testrails has something more suitable.

