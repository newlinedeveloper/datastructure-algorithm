### Intervals


#### Insert Interval

```
package main

import (
	"fmt"
	"sort"
)

type Interval struct {
	Start int
	End   int
}

func insert(intervals []Interval, newInterval Interval) []Interval {
	mergedIntervals := []Interval{}
	n := len(intervals)

	// Find the correct position to insert the newInterval
	i := 0
	for i < n && intervals[i].End < newInterval.Start {
		mergedIntervals = append(mergedIntervals, intervals[i])
		i++
	}

	// Merge intervals that overlap with the newInterval
	for i < n && intervals[i].Start <= newInterval.End {
		newInterval.Start = min(newInterval.Start, intervals[i].Start)
		newInterval.End = max(newInterval.End, intervals[i].End)
		i++
	}

	mergedIntervals = append(mergedIntervals, newInterval)

	// Append remaining intervals
	for i < n {
		mergedIntervals = append(mergedIntervals, intervals[i])
		i++
	}

	return mergedIntervals
}

func min(a, b int) int {
	if a < b {
		return a
	}
	return b
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}

func main() {
	intervals := []Interval{{1, 3}, {6, 9}}
	newInterval := Interval{2, 5}

	result := insert(intervals, newInterval)
	fmt.Printf("Merged intervals: %+v\n", result)
}


```

In this code, the Interval struct represents an interval with a start and end value. The insert function takes a slice of intervals and a new interval as input and returns the merged intervals after inserting the new interval.

The insert function follows the following steps:

Initialize an empty mergedIntervals slice to store the merged intervals.
Iterate through the existing intervals and insert them into mergedIntervals until reaching the correct position to insert the new interval. The correct position is determined based on the end value of each interval.
Merge intervals that overlap with the new interval. Update the start and end values of the new interval accordingly.
Append the new interval to mergedIntervals.
Append any remaining intervals from the original slice.
Return the mergedIntervals slice.
In the main function, we create an example slice of intervals intervals and a new interval newInterval. We call the insert function with these inputs and print the merged intervals.

Please note that the code assumes valid input, and additional error handling or checks may be required based on specific requirements.


#### Merge Intervals

```
package main

import (
	"fmt"
	"sort"
)

type Interval struct {
	Start int
	End   int
}

func merge(intervals []Interval) []Interval {
	if len(intervals) <= 1 {
		return intervals
	}

	// Sort intervals by start time
	sort.Slice(intervals, func(i, j int) bool {
		return intervals[i].Start < intervals[j].Start
	})

	mergedIntervals := []Interval{intervals[0]}

	for i := 1; i < len(intervals); i++ {
		current := intervals[i]
		previous := mergedIntervals[len(mergedIntervals)-1]

		// If the current interval overlaps with the previous merged interval, merge them
		if current.Start <= previous.End {
			previous.End = max(current.End, previous.End)
		} else {
			mergedIntervals = append(mergedIntervals, current)
		}
	}

	return mergedIntervals
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}

func main() {
	intervals := []Interval{{1, 3}, {2, 6}, {8, 10}, {15, 18}}
	merged := merge(intervals)
	fmt.Printf("Merged intervals: %+v\n", merged)
}


```

In this code, the Interval struct represents an interval with a start and end value. The merge function takes a slice of intervals as input and returns the merged intervals.

The merge function follows the following steps:

Sort the intervals by start time using the sort.Slice function and a custom comparison function.
Initialize an empty mergedIntervals slice to store the merged intervals. Add the first interval from the sorted input to mergedIntervals.
Iterate through the remaining intervals. For each interval, check if it overlaps with the last merged interval. If it does, update the end value of the last merged interval to cover both intervals. If it doesn't overlap, add the current interval to mergedIntervals.
Return the mergedIntervals slice.
In the main function, we create an example slice of intervals intervals. We call the merge function with this input and print the merged intervals.

Please note that the code assumes valid input, and additional error handling or checks may be required based on specific requirements.


#### Non Overlapping Intervals

```
package main

import (
	"fmt"
	"sort"
)

type Interval struct {
	Start int
	End   int
}

func eraseOverlapIntervals(intervals []Interval) int {
	if len(intervals) <= 1 {
		return 0
	}

	// Sort intervals by end time
	sort.Slice(intervals, func(i, j int) bool {
		return intervals[i].End < intervals[j].End
	})

	count := 0
	end := intervals[0].End

	for i := 1; i < len(intervals); i++ {
		// If the current interval overlaps with the previous interval, remove the current interval
		if intervals[i].Start < end {
			count++
		} else {
			end = intervals[i].End
		}
	}

	return count
}

func main() {
	intervals := []Interval{{1, 2}, {2, 3}, {3, 4}, {1, 3}}
	removed := eraseOverlapIntervals(intervals)
	fmt.Printf("Number of intervals removed: %d\n", removed)
}


```
In this code, the Interval struct represents an interval with a start and end value. The eraseOverlapIntervals function takes a slice of intervals as input and returns the minimum number of intervals that need to be removed to make the remaining intervals non-overlapping.

The eraseOverlapIntervals function follows the following steps:

Sort the intervals by end time using the sort.Slice function and a custom comparison function.
Initialize a count variable to keep track of the number of intervals to be removed and an end variable to store the end time of the first interval.
Iterate through the intervals starting from the second interval. If the start time of the current interval is less than the end time of the previous interval, it means they overlap, so increment the count. Otherwise, update the end variable to the end time of the current interval.
Return the count, which represents the minimum number of intervals to be removed.
In the main function, we create an example slice of intervals intervals. We call the eraseOverlapIntervals function with this input and print the number of intervals removed.

Please note that the code assumes valid input, and additional error handling or checks may be required based on specific requirements.


#### Meeting Rooms

```
package main

import (
	"fmt"
	"sort"
)

type Interval struct {
	Start int
	End   int
}

func canAttendMeetings(intervals []Interval) bool {
	if len(intervals) <= 1 {
		return true
	}

	// Sort intervals by start time
	sort.Slice(intervals, func(i, j int) bool {
		return intervals[i].Start < intervals[j].Start
	})

	for i := 1; i < len(intervals); i++ {
		// If the start time of the current interval is before the end time of the previous interval, return false
		if intervals[i].Start < intervals[i-1].End {
			return false
		}
	}

	return true
}

func main() {
	intervals := []Interval{{0, 30}, {5, 10}, {15, 20}}
	canAttend := canAttendMeetings(intervals)
	fmt.Printf("Can attend meetings: %t\n", canAttend)
}


```

In this code, the Interval struct represents an interval with a start and end value. The canAttendMeetings function takes a slice of intervals as input and returns a boolean indicating whether it is possible to attend all the meetings without any overlap.

The canAttendMeetings function follows the following steps:

Sort the intervals by start time using the sort.Slice function and a custom comparison function.
Iterate through the intervals starting from the second interval. If the start time of the current interval is before the end time of the previous interval, it means there is an overlap, so return false.
If no overlaps are found, return true.
In the main function, we create an example slice of intervals intervals. We call the canAttendMeetings function with this input and print whether it is possible to attend all the meetings without any overlap.

Please note that the code assumes valid input, and additional error handling or checks may be required based on specific requirements.



#### Meeting Rooms II

```
package main

import (
	"fmt"
	"sort"
)

type Interval struct {
	Start int
	End   int
}

func minMeetingRooms(intervals []Interval) int {
	if len(intervals) == 0 {
		return 0
	}

	startTimes := make([]int, len(intervals))
	endTimes := make([]int, len(intervals))

	for i, interval := range intervals {
		startTimes[i] = interval.Start
		endTimes[i] = interval.End
	}

	sort.Ints(startTimes)
	sort.Ints(endTimes)

	usedRooms := 0
	endIdx := 0

	for i := 0; i < len(intervals); i++ {
		if startTimes[i] < endTimes[endIdx] {
			usedRooms++
		} else {
			endIdx++
		}
	}

	return usedRooms
}

func main() {
	intervals := []Interval{{0, 30}, {5, 10}, {15, 20}}
	rooms := minMeetingRooms(intervals)
	fmt.Printf("Minimum number of meeting rooms required: %d\n", rooms)
}


```

In this code, the Interval struct represents an interval with a start and end value. The minMeetingRooms function takes a slice of intervals as input and returns the minimum number of meeting rooms required to schedule all the meetings without any overlap.

The minMeetingRooms function follows the following steps:

Create two slices, startTimes and endTimes, to store the start and end times of the intervals respectively.
Sort both startTimes and endTimes in ascending order.
Initialize usedRooms to 0 and endIdx to 0.
Iterate through the startTimes slice. If the start time of the current interval is less than the end time of the meeting that ended earliest, increment usedRooms. Otherwise, increment endIdx to consider the next meeting that ended earliest.
Return the value of usedRooms as the minimum number of meeting rooms required.
In the main function, we create an example slice of intervals intervals. We call the minMeetingRooms function with this input and print the minimum number of meeting rooms required.

Please note that the code assumes valid input, and additional error handling or checks may be required based on specific requirements.
