---
tags: [hard, graph, depth_first_search]
---
### 332. Reconstruct Itinerary 

Link: [here](https://leetcode.com/problems/reconstruct-itinerary/description/)

#### Problem
You are given a list of airline `tickets` where `tickets[i] = [fromi, toi]` represent the departure and the arrival airports of one flight. Reconstruct the itinerary in order and return it.

All of the tickets belong to a man who departs from `"JFK"`, thus, the itinerary must begin with `"JFK"`. If there are multiple valid itineraries, you should return the itinerary that has the smallest lexical order when read as a single string.

- For example, the itinerary `["JFK", "LGA"]` has a smaller lexical order than `["JFK", "LGB"]`.

You may assume all tickets form at least one valid itinerary. You must use all the tickets once and only once.

#### Main Idea
- Build an adjacency list from the tickets so you can traverse a graph of flights
- DFS starting from `'JFK'` and visit all neighbors
- Sort adjacency list data so it's in lexical order
- Once we traverse an edge on the graph we remove it by popping so that we don't revisit the same place/get caught in a cycle

#### Approach
- Build adjacency list of sources and destinations
- Sort the order of each adjacency list value array
- Make a DFS method that looks at the edges leaving from the current node, and for each edge, pops the edge and then travels to that destination
- Once we know we checked all neighbors, we can append the current node to the itinerary, since we have to include all elements in the result, and have fully processed that destination

#### Edge Cases
- Lots of room for issues with the tickets, invalid destinations, impossible legs, repeat trips, disconnected graphs, etc. 

#### Solution
```python 
lass Solution(object):
    def findItinerary(self, tickets):
        """
        :type tickets: List[List[str]]
        :rtype: List[str]
        """
        self.flightMap = defaultdict(list)

        for ticket in tickets:
            origin, dest = ticket[0], ticket[1]
            self.flightMap[origin].append(dest)

        # sort the itinerary based on the lexical order
        for origin, itinerary in self.flightMap.items():
        # Note that we could have multiple identical flights, i.e. same origin and destination.
            itinerary.sort(reverse=True)

        self.result = []
        self.DFS('JFK')

        # reconstruct the route backwards
        return self.result[::-1]

    def DFS(self, origin):
        destList = self.flightMap[origin]
        while destList:
            #while we visit the edge, we trim it off from graph.
            nextDest = destList.pop()
            self.DFS(nextDest)
        self.result.append(origin)```

#### Time Complexity
`O(n)` process all elements of the input, convert it to a graph, and then traverse said graph without dealing with cycles or repeats

#### Space Complexity
`O(n)` we make a graph data structure to represent the input (and we also make recursive calls so could potentially end up with a call stack of size `n`)

