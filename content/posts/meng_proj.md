---
title: "Robot Apocalypse One Step Closer: Robots Will No Longer Get Lost In Multi-Floor Buildings"
date: 2021-08-15
lastmod: 2021-08-15
draft: true
tags: ["robotics"]
---

When people ask me "what was your MEng thesis project," the one-sentence summary is "an algorithm for multi-floor robot path planning that has the same completeness and optimality guarantees as the underlying within-floor algorithm".  But if you've never studied robot path planning algorithms (a fair and wise decision), this probably doesn't mean a lot to you.  Let me break it down.

## Let's talk about path planning algorithms

So, first off, what's the state of the art?

I'm assuming we have a floor-based wheeled robot, and that it uses a 2D map to navigate in each floor, where each pixel represents a little square of space, which is either occupied (black) or unoccupied (white).  To make collision-checking easier, I'm also assuming that the map is a representation of the robot's __configuration space__, rather than physical space.  

With a robot that has, say, an arm, and therefore a lot of degrees of freedom, configuration space is a complex N-dimensional space where each point represents one possible "configuration" that the robot could have, specifying the position of each joint.  A free point in configuration space is a configuration you can reach without any bits of the arm colliding with anything.  Computing configuration space is hard in general, but with a wheeled robot that drives around on a floor and is more or less round, you can pretty much approximate it by taking a map of physical ("task") space and inflating all the obstacles by the robot's radius.

Basically, what this cashes out to is: If a pixel in my 2D configuration-space map is white, it maps 1:1 to a physical place in the real world that my robot could exist in without colliding with anything.

Once you have this configuration-space map, the path-planning problem consists of drawing a curve on this map that goes from a starting configuration to an ending configuration and moves through only unoccupied (white) parts of the map (and which the robot can actually execute -- this gets tricky if your robot is weirdly shaped (long and thin) or even just an Ackermann-steering platform (a car), but if it's more or less round and can stop and turn in any direction it needs to, whenever it needs to, pretty much any scribble you draw on the config-space map that doesn't intersect with any occupied space will work).

Taking a step back for a minute, let's talk about finding shortest paths in graphs.

[TODO: insert picture.  No, not these kinds of graphs.]

I'm going to assume you're acquainted with graphs in the sense of "a set of vertices, and a set of edges that connect those vertices, which may be labeled with 'weights' or 'costs'".  If you have a start vertex and a goal vertex, and you want to find the shortest path between them (the set of edges forming a path connecting the start and the goal that minimizes total cost), you probably want to use [Dijkstra's algorithm](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm).

[TODO: insert picture: It's an older algorithm, sir, but it checks out.]

The concept here is that we keep a priority queue of vertices, ordered based on the minimum cost to get to them from the start vertex, and at every iteration of the algorithm we remove from the queue the vertex with the lowest cost (let's call it Steve) and "explore" all the vertices it's adjacent to, in order to see if we can get to any of these neighbor vertices more cheaply if we go through Steve first.  For more details, the Wikipedia article I linked above has a thorough walkthrough of the pseudocode and some nice pictures.

We can apply Dijkstra's algorithm to the path planning problem if we consider every point in the configuration space to be a vertex, with edges connecting it to all of its neighboring points whose cost is identical to their physical length.  However, as this gif shows...

[TODO: insert gif of Dijkstra's algorithm from Wikipedia here]

...it's really slow!  And it wastes a bunch of time by exploring in the exact opposite direction from the goal.

The problem is that this graph we've created based on the config-space map doesn't capture a lot of relevant information about how vertices are related to each other spatially.  Dijkstra's algorithm assumes we don't know anything about the costs of edges or paths that we haven't yet explored, and there's every chance that if we go in the exact opposite direction from the goal that there might be a super-low-cost edge that takes us straight there.  But if we know we're operating in a real, physical plane, we're fairly sure that won't happen!  In fact, given any point in the configuration space, we know that the cost to get from that point to the goal vertex can't possibly exceed the physical distance from that point to the goal as the crow flies (Euclidean distance).

So we can improve on Dijkstra's algorithm by adding a *heuristic* -- an estimate on how much more cost it will incur to get to the goal vertex from our current vertex, Steve.  This gives us [A* search](https://en.wikipedia.org/wiki/A*_search_algorithm) -- now, our priority queue is ordered based on the cost to get from the start vertex to Steve plus the estimated cost to get from Steve to the goal.

[TODO: insert a* gif from wikipedia]

And it's a lot faster!  But we're still exploring every single pixel in the map between the start and the goal.  To give you a sense of scale, with the standard scale that [ROS](https://www.ros.org/) uses for maps, each pixel represents a 5cm by 5cm space.  Even a small office is going to require a 5,806,440 pixel map!

How can we speed this up?  Well... A* search is guaranteed to find the best possible path, and it's been proven that you can't find an equivalent algorithm that looks at fewer vertices.  So the only way we're going to be able to improve on this is by removing some of these guarantees.

RRT is an algorithm that, on its face, seems a lot worse than A*.  It doesn't at all guarantee you an optimal path -- in fact, its paths will often be pretty wiggly -- and it's randomized, so there's a small probability it won't even find the goal.

[TODO: insert gifs of a* vs. rrt]

On the other hand, it's really fast!  RRT stands for "rapidly-exploring random trees" -- you expand the "tree" by randomly selecting a point in configuration space, finding the closest point in the tree (which initially will just be the starting point), drawing a line from that point in the tree towards your randomly selected point, checking if the line goes through any obstacles, and -- if it doesn't -- adding it to the tree.

Most of the time, RRT will be a lot faster than A*; if you let it run for an arbitrarily long time, and a path to the goal exists, it'll find it with probability approaching 1.  (The fancy term for this guarantee is "probabilistic completeness").  But here's the rub:  if a path to the goal __doesn't__ exist, RRT won't tell you about it.  A* will terminate after a bounded time (based on the size of the graph), and will tell you either "I found a path" or "A path doesn't exist".  RRT will either terminate, and tell you "I found a path", or keep running forever, in which case all you know is that it hasn't found a path... __yet__.

The other annoying thing about RRT is that it's non-optimal; there's a version called RRT* that "rewires" the tree every so often to straighten out the paths, so if you let it run for an arbitrarily long time, you'll also get a path that asymptotically approaches the optimal length ("asymptotic optimality").  Like RRT, it's also probabilistically complete.

[TODO: gif of rrt*]

There's a bunch of variations of RRT* that try to be smarter about the way they select points to expand the tree, but we're just going to stick with vanilla RRT*.  It's pretty popular because it's fast even in high-dimensional spaces, for reasons I'm not going to go into here.  For now, the thing to keep in mind is that if we have a *single* 2D map, we can find a path within it using this algorithm, RRT*, that will either say "yes, I found a path," or "I haven't found a path yet but I'll keep looking"; if it's found a path, and you let it keep going long enough, eventually the path will get arbitrarily close to optimal.

## So what's the deal with multi-floor environments?

Uh, well, if what we have is a single 2D floor map, we can't exactly use that to represent a building that has, like, elevators.  We're going to need a slightly more complicated data structure.

My work, like most of the (fairly limited) literature on multi-floor planning, uses a set of 2D maps connected in a topological graph.  In every place you have an elevator or some other structure that connects two floors, you plonk down a vertex in each of the corresponding maps and connect them by an edge.  You also place edges between each pair of vertices in a floor.  It'll look something like this:

[TODO: insert picture]

The simple thing to do is to take this graph and apply Dijkstra's algorithm to it, which gets you a set of edges, which can be either within-floor or between-floor.  A between-floor edge means the robot has to ride an elevator, which is a tricky problem that I've totally assumed away -- I assume that these elevator transitions are perfectly deterministic and reliable, which generally gets big laughs when I tell this to other robotics engineers.  A within-floor edge, though, represents a planning problem within a single 2D floor map.  We know how to solve this!  You just feed every edge through RRT*, and if they all represent possible paths through the floor, you're good.

If even one of those edges is impossible to solve for some reason -- say, the floor is completely split down the middle by a wall, with elevator banks on each side, and since one paragraph ago we placed edges between each pair of vertices in the floor, there'll be an unsolvable edge between the two elevator banks -- you're fucked.  RRT* will just keep going forever.  You can ask it "hey, what's taking you so long?" but it can't tell you "dude this edge is impossible why did you give me this," it can only shrug and say "I dunno man I just haven't found a solution yet, I'll keep expanding the tree."

When in fact there may be a perfectly good solution to the __overall__ problem of going through the building, it just needs to take a different route through the overall topological graph, and the simple algorithm we use here has no way of kicking things back up the chain to Dijkstra's algorithm and asking it for a different path through the graph.  What we need is some kind of mechanism for going "wow, this edge is taking a lot longer than I feel like it ought to.  Let's put a pin in it for now, mark it as temporarily unusable, and find a new path through the version of the topological graph that doesn't have this edge in it."  __At the same time,__ the edge might genuinely just be super slow, but if you let RRT* chew on it for long enough it'd come up with a solution that's way more optimal than any other path through the topological graph -- so if we want to guarantee asymptotic optimality we can't actually remove it forever.

## What my algorithm does

My algorithm is split into two phases.  In the first phase, we try to find a path -- any path -- through the topological graph, as quickly as possible.  This is the phase where we guarantee probabilistic completeness.  In the second phase, we use any remaining planning time to improve on that path, both by trying different routes through the graph and by devoting extra time to all the edges in the path; this is what guarantees asymptotic optimality.

I kind of gave away the first phase already, but here it is:

1. Use our good old friend Dijkstra's algorithm to find a path through the topological graph.
2. Try to use RRT* on each edge in this path, but give it a timeout so that it can't take too long.
3. Any edge that we successfully solved, we won't replan again in Phase 1.  The rest of the edges -- that RRT* couldn't solve in the given timeout -- are temporarily removed from the topological graph.
4. Go back to 1.
5. If at any point Dijkstra's algorithm is like "oi, there's no longer a path through this graph," double the timeout and add back in all the edges we couldn't solve earlier.

The core concept here is inspired by [exponential backoff](https://en.wikipedia.org/wiki/Exponential_backoff) -- if we can't solve an edge in the given timeout, next time we replan it we'll give it twice as much time.  If there exists a path through the topological graph consisting only of solvable edges, we're guaranteed to find it eventually; given overall planning time approaching infinity, we're guaranteed to devote planning time approaching infinity to solving each edge, and therefore the probability of finding and solving the overall path approaches 1.

Okay, at the end of phase 1, we've found a path!  It might be suboptimal in two ways -- first, and most obviously, each of the individual edges might be suboptimal.  Second, there might be a better path through the topological graph that we haven't found yet, or that we don't yet know is better because we haven't put in enough planning time.

Luckily, we can do better than retrying all the edges an equal amount using some heuristics.  There are a bunch of edges in the topological graph that we don't care about, because they either aren't part of any path from the start vertex to the goal vertex, or they're part of a path that can't possibly be any shorter than the one we've already found.  (Just like with A*, each edge has a lower-bound length: the Euclidean distance between its two endpoints.)  We'll only replan edges that are part of a path whose lower-bound length is less than the planned length of the path we've already found (note that this will include all edges in the current path).

Phase 2 goes like this:

1. Enumerate all the paths in the topological graph by lower-bound length.
2. Stick all the edges we care about (edges that are part of a path with lower-bound length less than the shortest planned path we've found so far) in a replan set and ignore the rest.
3. Feed every edge in the replan set into RRT*, with some timeout.
4. Using the new edge lengths, find the new shortest path and its total length.
5. Repeat from 2, and keep going until you run out of planning time.

## BUT DOES IT WORK

It does!!  I tested it on the MIT campus, because 1) MIT is big and has lots of multi-floor buildings and 2) I could download all the floorplans from the Facilities website.

MIT's campus has an extensive set of basements and underground tunnels, such that you, or a robot, can get from one side of the main campus to the other entirely through basements and without going up any stairs.  I used this as one of my primary test cases, because it's really fuckin' big.  But I also wanted to test things out on a multifloor building.  My candidate here was the Stata Center; it was designed by Frank Gehry, and my advisor Leslie Kaelbling says that it's the counterexample to every robot paper.

(I knew about some of the fucked-up elevators in Stata before working on this project, but in the course of preparing all the floorplans for use as robot maps, I discovered several more.  There's an elevator on the first floor whose purpose I was never able to figure out, because there isn't a corresponding elevator or even an elevator shaft in the basement or on the second floor, and it seems to be buried in the bowels of the first-floor cafe.  But it's clearly labeled as an elevator on the floorplans!!  If you're on MIT's campus and you figure out what this elevator's purpose is, PLEASE let me know.  This is haunting me.)

[TODO: talk about results]

There's one other benefit I didn't anticipate -- under the right circumstances, if you have a really big 2D floor and you split it up right, my algorithm is significantly faster than RRT* on the full floor map.  As a point of comparison, I smashed all the basement maps together in Photoshop to make one enormous 2D map of the entire basement.  RRT* on this map took [TODO: time]; my algorithm managed an average of [TODO: time].

I think this is partly because RRT* has a known tendency to be really slow at dealing with bottlenecks, like narrow doorways or hallways.  Since most of the transition points between buildings are at these kinds of chokepoints, my algorithm naturally ends up giving RRT* a big hint about how to get through them.

Another reason is that the full composite map has huge quantities of space outside any of the buildings; I filled these all in with a dark gray that my collision checker also interpreted as "occupied".  This means that on the big map, many of the RRT* samples that it uses to grow the tree will fall into the dark gray void, which is not very useful.  On the small maps, there's just as much unoccupied space total, but a lot less void.

## Further reading

My thesis will... eventually... be available on dspace.mit.edu, but for now you can look at it here [TODO: host thesis as pdf].  See the bibliography for more papers about the underlying planning algorithms I used!

The source code for my test implementation is on my GitHub [here](https://github.com/shilohc/meng-proj).  Judging by my commit messages there are still some bugs but I can no longer remember what they are, so watch out.

