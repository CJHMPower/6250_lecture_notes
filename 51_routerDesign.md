## Questions

## Test Prep
1. Describe the basic functions any router on the Internet must be able to perform in order to route traffic. What are some of the hardware features
   present on modern routers that support these functions?
  - The main function of a router is to take a packet as input and forward it to the correct destination port(output-interface)
  - The interconnection fabric allows all the linecards to connect to each other
  - Each line card has a lookup table
    - This prevents a central lookup table from becoming a bottleneck

2. Describe the purpose of a Maximal Matching switching algorithm commonly used to schedule traffic on router crossbars.
  - The purpose of maximal matching is to get the most efficiency possible during each timeslot. You ideally would like a 1 to 1 mapping of inputs to
    outputs

3. How would a router schedule bandwidth for a single timeslot (1s) for the following forwarding demands using a Max-Min Fairness algorithm (in Mb):
   1.8, 2.4, 3.0, 5.3, 6.4, 1.0, 2.2, 8.0? Assume the algorithm can forward at most 25.6 Mb of data in a single timeslot.

--------------

#### Basic Router Architecture
  1. Receive packet
  2. Look at packet header to determine destination
  3. Look in forwarding table to determine appropriate output interface
  4. Modify the packet header (i.e. TTL, updating IP header checksum)
  5. Send packet to correct port/output-interface

  - Generally speaking, there are multiple line cards that are connect to each other via an *interconnection fabric*
  - Each line card has:
    a. its own lookup table
    b. the ability to modify packet headers
    c. a queue

  - Placing a lookup table removes the bottleneck of having a central lookup table

#### Crossbar Switch
  - Every input port has a connection to every output port
  - There are set timeslot where each input can be connected to zero outputs or 1 output
  - Allows for parallelism
    - requires a proper scheduling algorithm

#### Maximal Matching
  - `n` inputs and `n` outputs
  - ideally, you would like a 1-to-1 mapping of inputs and outputs during each timeslot (maximally matched)

#### Head of line blocking
  - this happens when packets get queued far behind other packets destined for other ports
  - the solution is to create virtual queues
    - This is basically just 1 queue per output port

#### Max/Min Fairness
  - imagine a flow allocation `[x1, x2, ..., xn]`
  - the axiom is that increasing any `xi` requires decreasing some other `xj` where `xj < xi`
  - So **small** demands get allocated the exact amount they need, while the larger demands get the remaining bandwidth split among them
  - The best way to visualize this is to do the example problem

```python
flow_allocation = [1.8, 2.4, 3.0, 5.3, 6.4, 1.0, 2.2, 8.0]
bandwidth = 25.6 Mb

# divide total bw by array.length
per_user = bandwidth/flow_allocation.length
per_user = 3.2 Mb

# now allocate to all the ones that can be fulfilled with 3.2 Mb
fulfilled = [1.8, 2.4, 3.0, 1.0, 2.2]
unfulfilled = [5.3, 6.4, 8.0]

# the leftover from fulfilled is
leftovers = (3.2-1)+(3.2-1.8)+(3.2-2.2)+(3.2-2.4)+(3.2-3) # 5.6 Mb
split_leftovers = 5.6/unfulfilled.length # 1.866 Mb

# now allocate the remaining
remaining = 3.2 + 1.866 # 5.0667 Mb

# so the final is
fulfilled = [1.8, 2.4, 3.0, 1.0, 2.2]
allocated_unfulfilled = [5.07, 5.07, 5.07]
end_result = fulfilled.concat(allocated_unfulfilled) = 
split_leftovers = 5.6/unfulfilled.length # 1.866 Mb

# now allocate the remaining
remaining = 3.2 + 1.866 # 5.0667 Mb

# so the final is
fulfilled = [1.8, 2.4, 3.0, 1.0, 2.2]
allocated_unfulfilled = [5.07, 5.07, 5.07]
end_result = fulfilled.concat(allocated_unfulfilled) = [1.8, 2.4, 3.0, 1.0, 2.2, 5.07, 5.07, 5.07]
```
