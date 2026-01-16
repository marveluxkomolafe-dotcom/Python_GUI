import tkinter as tk
from tkinter import ttk, messagebox
import random
from datetime import datetime, timedelta

# Destination → Travel time (minutes)
travel_times = {
    "ibadan": 15,
    "ilorin": 25,
    "abuja": 45,
    "port harcourt": 40,
    "abeokuta": 9
}

# Destination → Base price (Naira)
base_prices = {
    "ibadan": 11000,
    "ilorin": 12900,
    "abuja": 15000,
    "port harcourt": 13500,
    "abeokuta": 5000
}

TOTAL_ROWS = 100
available_seats = [(row, seat) for row in range(1, TOTAL_ROWS + 1) for seat in range(1, 7)]


def generate_ticket():
    try:
        name = entry_name.get().strip()
        age = int(entry_age.get().strip())
        destination = combo_destination.get().lower()
        class_choice = combo_class.get().lower()
        dep_time = entry_time.get().strip()

        if not name:
            messagebox.showerror("Error", "Passenger name required.")
            return

        if age < 15:
            guardian_age = tk.simpledialog.askinteger("Guardian Check", "Passenger is under 15. Enter guardian age (18+):")
            if guardian_age is None or guardian_age < 18:
                messagebox.showerror("Error", "Cannot issue ticket: Guardian must be 18+")
                return

        if destination not in travel_times:
            messagebox.showerror("Error", "Destination not listed.")
            return

        # Class selection
        if class_choice == "first":
            travel_class = "First Class"
            surcharge = 10000
            row = random.randint(1, 20)
        elif class_choice == "business":
            travel_class = "Business Class"
            surcharge = 5000
            row = random.randint(21, 40)
        elif class_choice == "economy":
            travel_class = "Economy Class"
            surcharge = 0
            row = random.randint(41, 100)
        else:
            messagebox.showerror("Error", "Invalid class choice.")
            return

        # Seat assignment
        seat = random.randint(1, 6)
        if (row, seat) in available_seats:
            available_seats.remove((row, seat))
        else:
            seat = random.choice([s for r, s in available_seats if r == row])
            available_seats.remove((row, seat))

        # Pricing
        base_price = base_prices[destination]
        final_price = base_price + surcharge

        # Time parsing
        try:
            departure_time = datetime.strptime(dep_time, "%H:%M")
        except ValueError:
            messagebox.showerror("Error", "Invalid time format. Use HH:MM 24-hour.")
            return

        duration = travel_times[destination]
        arrival_time = departure_time + timedelta(minutes=duration)

        # Display ticket
        ticket_text = f"""
====================================
             TRAIN TICKET
====================================
Passenger Name : {name}
Age            : {age}
Destination    : {destination.title()}
Class          : {travel_class}
Row & Seat     : Row {row}, Seat {seat}
Ticket Price   : ₦{final_price}
------------------------------------
Departure Time : {departure_time.strftime('%H:%M')}
Arrival Time   : {arrival_time.strftime('%H:%M')}
Duration       : {duration} minutes
====================================
     WISHING YOU A SAFE JOURNEY!
====================================
"""

        text_output.delete("1.0", tk.END)
        text_output.insert(tk.END, ticket_text)

    except Exception as e:
        messagebox.showerror("Error", str(e))


# GUI window
root = tk.Tk()
broot = root
root.title("Train Ticket Issuer")
root.geometry("600x650")
root.resizable(False, False)

# Widgets
label_title = tk.Label(root, text="TRAIN TICKET ISSUER", font=("Arial", 18, "bold"))
label_title.pack(pady=10)

frame_form = tk.Frame(root)
frame_form.pack(pady=10)

# Name
label_name = tk.Label(frame_form, text="Passenger Name:")
label_name.grid(row=0, column=0, sticky="w")
entry_name = tk.Entry(frame_form, width=30)
entry_name.grid(row=0, column=1, padx=10, pady=5)

# Age
label_age = tk.Label(frame_form, text="Passenger Age:")
label_age.grid(row=1, column=0, sticky="w")
entry_age = tk.Entry(frame_form, width=30)
entry_age.grid(row=1, column=1, padx=10, pady=5)

# Destination
label_destination = tk.Label(frame_form, text="Destination:")
label_destination.grid(row=2, column=0, sticky="w")
combo_destination = ttk.Combobox(frame_form, values=list(travel_times.keys()), width=28, state="readonly")
combo_destination.grid(row=2, column=1, padx=10, pady=5)
combo_destination.current(0)

# Class
label_class = tk.Label(frame_form, text="Travel Class:")
label_class.grid(row=3, column=0, sticky="w")
combo_class = ttk.Combobox(frame_form, values=["First", "Business", "Economy"], width=28, state="readonly")
combo_class.grid(row=3, column=1, padx=10, pady=5)
combo_class.current(2)

# Departure time
label_time = tk.Label(frame_form, text="Departure Time (HH:MM):")
label_time.grid(row=4, column=0, sticky="w")
entry_time = tk.Entry(frame_form, width=30)
entry_time.grid(row=4, column=1, padx=10, pady=5)

# Button
btn_generate = tk.Button(root, text="Generate Ticket", font=("Arial", 12, "bold"), command=generate_ticket)
btn_generate.pack(pady=15)

# Output
text_output = tk.Text(root, width=70, height=20, font=("Courier", 10))
text_output.pack(pady=10)

root.mainloop()
