# Projects
def calculate_payment(day, start_time, hours):
    weekdays_rates = {'MO': 25, 'TU': 15, 'WE': 15, 'TH': 15, 'FR': 15}
    weekends_rates = {'SA': 30, 'SU': 30}
# Convert start time to hours and minutes

    start_hour, start_minute = map(int, start_time.split(':'))
    
    # Calculate the payment based on the day and time
    if day in weekdays_rates:
        if start_hour < 9:
            payment = min(hours, 9 - start_hour) * weekdays_rates[day]
            hours -= min(hours, 9 - start_hour)
            start_hour = 9
        if hours > 0 and start_hour < 18:
            payment += min(hours, 18 - start_hour) * weekdays_rates[day]
            hours -= min(hours, 18 - start_hour)
        if hours > 0:
            payment += hours * weekdays_rates[day]
    elif day in weekends_rates:
        if start_hour < 9:
            payment = min(hours, 9 - start_hour) * weekends_rates[day]
            hours -= min(hours, 9 - start_hour)
            start_hour = 9
        if hours > 0 and start_hour < 18:
            payment += min(hours, 18 - start_hour) * weekends_rates[day]
            hours -= min(hours, 18 - start_hour)
        if hours > 0:
            payment += hours * weekends_rates[day]
    else:
        return "Invalid day"
    
    return payment

# Read input from a text file
with open('employee_schedule.txt', 'r') as file:
    lines = file.readlines()

# Process each set of data
for line in lines:
    data = line.strip().split()
    employee_name = data[0]
    day = data[1]
    start_time = data[2]
    hours_worked = float(data[3])
    
    payment = calculate_payment(day, start_time, hours_worked)
    
    print(f"Employee: {employee_name}\tPayment: {payment} USD")
