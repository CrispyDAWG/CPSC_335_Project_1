# Convert string to int -- the int being minutes since midnight
def clock_to_min(time_str):
    hours = int(time_str.split(':')[0])
    minutes = int(time_str.split(':')[1])
    return hours * 60 + minutes


# Convert array of clock times(strings) to array of minutes(integers)
def convert_schedule_to_minutes(schedule):
    converted = []
    for time in schedule:
        start, end = time
        converted.append([clock_to_min(start), clock_to_min(end)])
    return converted


# Insertion sort function to sort intervals based on start time
def ins_sort(intervals):
    for i in range(1, len(intervals)):
        key = intervals[i]
        j = i - 1
        while j >= 0 and intervals[j][0] > key[0]:
            intervals[j + 1] = intervals[j]
            j -= 1
        intervals[j + 1] = key
    return intervals


# Merge overlapping intervals
def merge_intervals(intervals):
    if not intervals:
        return []
    # Sort intervals using ins_sort
    sorted_intervals = ins_sort(intervals)
    merged = [sorted_intervals[0]]
    for current in sorted_intervals[1:]:
        last = merged[-1]
        # If overlap
        if current[0] <= last[1]:
            last[1] = max(last[1], current[1])  # Merge intervals
        else:
            merged.append(current)  # No overlap-- add new interval
    return merged


# Find available intervals between busy times
def find_available_intervals(busy_intervals, start, end):
    available_intervals = []
    prev_end = start
    for start_time, end_time in busy_intervals:
        if start_time > prev_end:  # There is a gap add to array
            available_intervals.append([prev_end, start_time])
        prev_end = max(prev_end, end_time)
    if prev_end < end:  # Check for available times after events
        available_intervals.append([prev_end, end])
    return available_intervals


# make sure times are compliant with set meeting duration
def duration_compliant(times, duration):
    filtered_times = []
    for time in times:
        start, end = time
        if end - start >= duration:
            filtered_times.append(time)
    return filtered_times


# Merge two schedules into one
def merge_schedules(schedule1, schedule2):
    return schedule1 + schedule2


# Convert int back to string -- the string being clock time
def min_to_clock(minutes):
    hours = minutes // 60
    mins = minutes % 60
    return f"{hours:02d}:{mins:02d}"


# Convert minutes array to clock time array
def convert_min_to_clock(minutes):
    converted_times = []
    for min in minutes:
        start = min_to_clock(min[0])
        end = min_to_clock(min[1])
        converted_times.append([start, end])
    return converted_times


def main():
    # Sample input:
    #                     *** change input here to see different results ***
    ######################################################################################################
    person1_schedule = [['7:00', '8:30'], ['12:00', '13:00'], ['16:00', '18:00']]
    person1_daily_act = ['9:00', '19:00']

    person2_schedule = [['9:00', '10:30'], ['12:20', '13:30'], ['14:00', '15:00'], ['16:00', '17:00']]
    person2_daily_act = ['9:00', '18:30']

    duration_of_meeting = 30  # len of meeting
    ######################################################################################################

    # Convert both peoples schedules to minutes
    person1_schedule_minutes = convert_schedule_to_minutes(person1_schedule)
    person2_schedule_minutes = convert_schedule_to_minutes(person2_schedule)

    # Convert daily act time to minutes
    person1_daily_act_minutes = [clock_to_min(person1_daily_act[0]), clock_to_min(person1_daily_act[1])]
    person2_daily_act_minutes = [clock_to_min(person2_daily_act[0]), clock_to_min(person2_daily_act[1])]

    # Merge schedules
    busy_intervals = merge_schedules(person1_schedule_minutes, person2_schedule_minutes)

    # Merge overlap
    merged_busy_intervals = merge_intervals(busy_intervals)

    # Find what is earliest they can both start and latest they can work
    common_start = max(person1_daily_act_minutes[0], person2_daily_act_minutes[0])
    common_end = min(person1_daily_act_minutes[1], person2_daily_act_minutes[1])

    # Find available times
    available_times = find_available_intervals(merged_busy_intervals, common_start, common_end)

    # Make sure intervals are compliant with duration
    compliant_times = duration_compliant(available_times, duration_of_meeting)

    # Change back to clock format
    meeting_times = convert_min_to_clock(compliant_times)

    print(meeting_times)


if __name__ == "__main__":
    main()
