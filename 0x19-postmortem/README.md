The Leap Second Glitch

Date: Occurred between June 30th, 2012 - July 1st, 2012.

Duration of Outage: 23:59:59 - 01:40:00 (UTC) between June 30th - July 1st.

Impact: Servers became extremely slow and eventually went down, causing the site (reddit.com) to go offline.

- 100% of users experienced slow response times from our website.

- The issue was first detected around midnight on June 30th.

- We eventually had to reboot all servers to clear the issue, which took about 40 minutes. The website was completely offline during this time.

Total Affected User Time: 1 hour of slow servers, followed by a 40-minute complete outage, totaling 1 hour and 40 minutes.

Detection: Customer complaints on the website alerted engineers on-call. The issue was then escalated to Internal Server Engineers.

Initial Hypothesis: The issue was initially thought to originate from AWS, as they also experienced an outage during this time.

Resolution: The incident was resolved by rebooting the servers.

Detailed Breakdown of the Issue:

This problem affected server infrastructures that run on top of Linux and Java. Internally, a subsystem called `hrtimer` handles high-resolution timers (nano-millisecond timing) that "wake up" sleeping processes using alarms. `hrtimer` interacts with the Network Timing Protocol (NTP), which syncs with the world atomic clocks to keep in sync with world time.

Because the Earth's rotation is not consistently 24 hours, occasionally 1 second is added to world clocks to make up for this inconsistency. The leap second is accommodated by delaying the world atomic clocks by 1 second. When this happens, `hrtimer` instantly becomes 1 second ahead, firing several sleeping processes all at once and prematurely. This overloads the server's CPU, causing a slowdown or crash. The total outage of the site occurred during the rebooting time, and rebooting the servers reset all subsystems and resolved the issue.

Corrective and Preventative Measures:

1. Pausing the system clock to avoid the sudden leap.

2. Spreading the extra second over a long period of time so the system doesn’t jump.

3. A patch for this problem has been provided by John Stultz.

TODO:

- Reproduce the Leap Second Problem to test possible solutions.

- Experiment with shutting down servers for 5 seconds during the leap second time.

- Try re-updating `hrtimer` on the addition of a leap second.
