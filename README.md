# password-manager
## Problem: Create and store a long and complicated password in one place!
## Solutions
1. All the libraries we need for this project
```
from tkinter import * # Tkinter is for Graphical User Interface (GUI)
from tkinter import messagebox # Messagebox will show the warning when the input fields are missing
import random # We will generate random passwords
import pyperclip # The program will automatically copy the randomly generated passwords
```
2. Password generator
```
def generate_password():
    letters = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z', 'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z']
    numbers = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9']
    symbols = ['!', '#', '$', '%', '&', '(', ')', '*', '+']

    nr_letters = random.randint(8, 10)
    nr_symbols = random.randint(2, 4)
    nr_numbers = random.randint(2, 4)

    # Change the existing for loops to use Python list comprehension instead
    password_list = [random.choice(letters) for char in range(nr_letters)]
    password_list += [random.choice(symbols) for char in range(nr_symbols)]
    password_list += [random.choice(numbers) for char in range(nr_numbers)]

    # Remember to combine the results so that you can shuffle them to create a password
    random.shuffle(password_list)
    password = "".join(password_list)
    password_entry.insert(END, password)

    # pyperclip library allows to copy the password into the clipboard
    pyperclip.copy(password)
```
3. Save the password
```
def save():
    """
    Write to the data inside the entries to a data.txt file when the Add button is clicked
    Each website, email, and pwd combination should be on a new line inside the file
    Save it to data.txt
    """
    website = website_entry.get()
    email = email_entry.get()
    password = password_entry.get()

    # Do not save the data and show the pop up above if the website or password fields were left empty
    if len(website) == 0 or len(password) == 0:
        messagebox.showinfo(title="Oops", message="Please don't leave any fields empty!")
    else:
        is_ok = messagebox.askokcancel(title=website, message=f"These are the details entered: \nEmail: {email} \nPassword: {password} \nIs it okay to save?")
        if is_ok:
            with open("data.txt", "a") as data:
                data.write(f"{website} | {email} | {password}\n")
                # All fields need to be cleared Add button is pressed
                website_entry.delete(0, 'end')
                password_entry.delete(0, 'end')
                
```
4. UI Setup
```
# Import tk inter
window = Tk()
window.title("Password Manager")
window.config(padx=50, pady=50)

# Canvas for the image of pomodoro
canvas = Canvas(width=200, height=200, highlightthickness=0)
logo_img = PhotoImage(file="logo.png")
canvas.create_image(100, 100, image=logo_img)
canvas.grid(row=0, column=1)

# Website labels and entries
website_label = Label(text="Website:")
website_label.grid(row=1, column=0)
website_entry = Entry(width=35)
website_entry.grid(row=1, column=1, columnspan=2)
website_entry.focus()

# Email labels and entries
email_label = Label(text="Email/Username:")
email_label.grid(row=2, column=0)
email_entry = Entry(width=35)
email_entry.grid(row=2, column=1, columnspan=2)
email_entry.insert(END, "billy@email.com")

# Password labels, button, and entries
password_label = Label(text="Password:")
password_label.grid(row=3, column=0)
password_entry = Entry(width=21)
password_entry.grid(row=3, column=1)
password_button = Button(text="Generate Password", command=generate_password)
password_button.grid(row=3, column=2)

# Add button
add_button = Button(text="Add", width=36, command=save)
add_button.grid(row=4, column=1, columnspan=2)

window.mainloop()
```
5. Save it to JSON file
```
try:
    with open("data.json", "r") as data_file:
        # Reading old data
        data = json.load(data_file)
except FileNotFoundError:
    with open("data.json", "w") as data_file:
        # Saving new data if file not found
        json.dump(new_data, data_file, indent=4)
else:
    # Updating old data with new data
    data.update(new_data)

    with open("data.json", "w") as data_file:
        # Saving updated data
        json.dump(data, data_file, indent=4)
finally:
    website_entry.delete(0, 'end')
    password_entry.delete(0, 'end')
```
6. Create a function called find_password() that gets triggered when the "Search" button is pressed
```
def find_password():
    """Create a function called find_password() that gets triggered when the "Search" button is pressed"""

    # Catch an exception that might occur trying to access the data.json showing a messagebox with the text "No Data File Found".
    try:
        # Read the data
        with open("data.json", "r") as data_file:
            # Reading old data
            data = json.load(data_file)

            # Check if the user's text entry matches an item in the data.json
            website = website_entry.get()
            email = data.get(website, {}).get('email')
            password = data.get(website, {}).get('password')

        # If yes, show a messagebox with the website's name and password
        if website in data:
            messagebox.showinfo(title=f"{website} Login Info", message=f"Email: {email}\nPassword: {password}")

        # If the user's website does not exist inside the data.json, show a messagebox that reads "No details for the website exists."
        else:
            messagebox.showinfo(title="Opps", message="No details for the website exists.")

    except FileNotFoundError:
        messagebox.showinfo(title="Opps", message="No Data File Found.")
```
## Lessons
1. The order of operation is from step 4-3-2-1
2. Break down the problem before solving one
3. Come back later if you get stuck
4. No rush, no pause ;)
