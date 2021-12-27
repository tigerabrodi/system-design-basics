# Key Characteristics of Distributed Systems

## Scalability

- Scalability is the concept of a system being able to manage growing amount of work by adding resources to the system. Ideally we want our systems to grow without the performance getting worse.

- Horizontal scaling means you add more servers into your pool of resources.

- Vertical scaling means adding more power (CPU, RAM, Storage, etc.) to an existing server.

- Horizontal scaling is almost always what we want, because we won't suffer from downtime compared to vertical scaling. If any servers are offline or crash, we can still be online due to having other servers, compared to vertical scaling where you only have one server.

- Horizontal scaling can also be called scaling out, and vertical scaling up.

- Examples of horziontal scaling are Cassandra and MongoDB. A vertical one would be MySQL, you can switch from smaller to bigger machines, but this process often involves downtime.

## Reliability

- A distributed system is realiable if it keeps delivering its services even when one or several of its software or hardware components fail.

- The probability that a system performs correctly during a specific time perod.

- If a machine fails, it should be able to be replaced and the requested task should still be able to get completed.

- This comes with a cost, since we need to eliminated every single point of failure, hence need extra machines.

## Availability

- Availability is the time a system remains operational to perform its required function in a specific perod.

- Take a plane for an example, it may fly a lot in a month with very little downtime, but during times it is in maintenance, it is not available. Now, a plane that has not been properly tested, is not reliable, and may not be able to fly in any type of weather, compared to a plane that has been tested and does not have any vulnerabilities.

- A reliable system is available, but an available system may not be reliable.

- If a system is reliable, it is available long-term. The other way around, a system may be available, but that doesn't mean it will stay like that for long.

- We can also take a software project into account. The difference between one that has been properly tested, and one that hasn't. The one that is properly tested, we can ensure every part of the software works as expected, hence if a bug was to occur, it'd likely be far from now considering the cases we have covered.

## Efficieny

- A system is efficient if it uses inputs in a proper way. Looking at the input-output ratio, if it is not good, then the system is inefficient.

## Serviceability or Manageability

- How easy it is to operate and maintain a distributed system.

- Simplicity and speed with which a system can be repaired or maintained.

- In this case testing helps a lot (the right ones, we don't want tests which break every time we change code), but also writing clean code and making sure that the architecture is good, i.e. keeping things isolate, modular and decoupled.

- It is also important to keep the test code on the same standard as the production code, making sure we delete tests we no longer need and also improve existing ones if possible.
