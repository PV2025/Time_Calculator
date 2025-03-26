# Time_Calculator

def add_time(start, duration, day=None):
    days_of_week = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']

    # Split start into time and period
    time_part, period = start.split()
    start_hour, start_minute = map(int, time_part.split(':'))

    # Convert start time to 24-hour format
    if period == 'PM' and start_hour != 12:
        start_hour += 12
    elif period == 'AM' and start_hour == 12:
        start_hour = 0

    # Parse duration
    duration_hour, duration_minute = map(int, duration.split(':'))

    # Add minutes and calculate carryover to hours
    end_minute = start_minute + duration_minute
    extra_hour = end_minute // 60
    end_minute = end_minute % 60

    # Add hours
    end_hour = start_hour + duration_hour + extra_hour
    days_later = end_hour // 24
    end_hour = end_hour % 24

    # Convert back to 12-hour format
    if end_hour == 0:
        display_hour = 12
        display_period = 'AM'
    elif end_hour < 12:
        display_hour = end_hour
        display_period = 'AM'
    elif end_hour == 12:
        display_hour = 12
        display_period = 'PM'
    else:
        display_hour = end_hour - 12
        display_period = 'PM'

    # Format minutes
    display_minute = f"{end_minute:02d}"

    # Start building the result string
    new_time = f"{display_hour}:{display_minute} {display_period}"

    # Handle day of the week if provided
    if day:
        start_day_index = days_of_week.index(day.capitalize())
        new_day_index = (start_day_index + days_later) % 7
        new_day = days_of_week[new_day_index]
        new_time += f", {new_day}"

    # Add day later text
    if days_later == 1:
        new_time += " (next day)"
    elif days_later > 1:
        new_time += f" ({days_later} days later)"

    return new_time
