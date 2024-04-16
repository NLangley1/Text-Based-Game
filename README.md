# Text-Based-Game
Spaceship style game
---Showcase---


rooms = {
    "Cafeteria": {
        "North": "Airlock",
        "West": "Terrarium",
        "East": "Dormitory",
        "item": None,
    },
    "Airlock": {"South": "Cafeteria", "item": "Alien"},
    "Terrarium": {"East": "Cafeteria", "item": "Oxygen"},
    "Dormitory": {"West": "Cafeteria", "South": "Medical Bay", "item": "Helmet"},
    "Medical Bay": {"North": "Dormitory", "South": "Engine Room", "item": "Medicine"},
    "Engine Room": {"North": "Medical Bay", "West": "Command Deck", "item": "Kapton"},
    "Command Deck": {
        "East": "Engine Room",
        "North": "Captains Quarters",
        "West": "Observation Deck",
        "item": None,
    },
    "Observation Deck": {"East": "Command Deck", "item": "Radio"},
    "Captains Quarters": {"South": "Command Deck", "item": "Pistol"},
}

DIRECTIONS = ["North", "South", "East", "West"]


# Define the intro of the game with user inputs
def intro():
    while True:
        answer = input("Would you like to play a game? (y/n) ")
        if answer.lower() == "y" or answer.lower() == "n":
            # Based on the rubric the player must win or lose but must play the game with no exit
            # If they answer 'y' or 'n' the game starts
            print('\nTrick question.\n')
            print(
                "\nThe year is 8264. You are on the USS Armageddon docked outside Mars. Your crew is on the surface \n"
                "currently collecting scientific research. You are the sole crewman aboard your vessel. Suddenly, \n"
                "you feel something large hit your ship. After you steady yourself, " 
                "you hear an alarm coming from the \n"
                "sensor on the outer airlock. You must go to investigate and quickly don your spacesuit." 
                "As you open \n"
                "the airlock, you see an 8-foot-tall creature with long sharp claws and teeth, "
                "it does not appear happy \n"
                "to see you... Before you can engage the outer door and send this alien flying, " 
                "it quickly reaches out \n"
                "its long slender arm and slashes your suit, breaking the seal "
                "on your helmet making it unusable. The \n"
                "creature lurches closer in hopes of devouring its lonely meal, but you quickly press the override \n"
                "control button and close the interior door, sealing it inside the airlock. You can see through the \n"
                "thick tempered glass window that the controls were disabled inside the airlock by the sheer size of \n"
                "the beast forcing its way inside, severing your ability to open the outer door.\n"
            )
            break
        else:
            print('\nDo you think we have time to mess around?\n')
    while True:
        choice = input("Do you want some help? (y/n) ")
        if choice.lower() == "y":
            print(
                "\nHmm... Maybe you will survive. You must find Kapton to seal your suit, "
                "some Medicine to heal yourself, \n"
                "a new Helmet and Oxygen to be able to breathe in case the beast forces its way inside the inner \n"
                "door. Find a radio to warn your crew and the pistol to save yourself and your ship from this \n"
                "uninvited guest.\n"
            )
            break
        elif choice.lower() == "n":
            print("\nSeriously? Okay, I will send your family flowers. Good luck...\n")
            break
        else:
            print("\nIts a 'y' or a 'n'. It is not that hard..\n")
    return True


# defining the instructions for the user
def show_instructions():
    print(
        "----Alien Survival--- \n"
        "Type Commands:\n"
        "go [North, South, East, West] \n"
        "get [item name] \n"
        "---------------------\n"
    )


# function to handle get for inventory
def getitem(current_loc, inventory, item):
    if item in inventory:
        print("Don\'t be greedy! You already have this!")
    elif item != rooms[current_loc]["item"]:
        print("What is that??")
    else:
        inventory.append(rooms[current_loc]["item"])
        print("Item retrieved!")
        return inventory


# Defining the room movement
def navigate(current_loc, direction):
    if direction in DIRECTIONS and direction in rooms[current_loc]:
        new_loc = rooms[current_loc][direction]
        return new_loc
    else:
        print("That is an invalid direction.")
        return current_loc


# Defining the loop that gives the player their status
def show_status(current_loc, inventory):
    print("You are in the " + current_loc)
    print("Inventory : " + str(inventory))
    if "item" in rooms[current_loc] and rooms[current_loc]["item"] is not None:
        print(f"You see \'{rooms[current_loc]['item']}\' in the room.\n")
    if current_loc == "Engine Room":
        print('Look Out! Whew that was close. That Alien keeps shaking the ship. Tread Carefully!\n')


# defining command for user to 'get' item
def get_command():
    user_input = input("What is your next move? ")
    command_list = user_input.split(" ", 1)
    if len(command_list) != 2:
        arg = " "
    else:
        arg = command_list[1]
    return command_list[0], arg


# defining how the game will work and play out based on user interaction
def main():
    current_loc = "Cafeteria"
    inventory = []

    intro()
    show_instructions()
    while True:
        show_status(current_loc, inventory)
        user_input = input("What is your next move? ").lower().split()
        if len(user_input) == 0:
            continue
        command = user_input[0]
        if command == "go" and len(user_input) > 1:
            current_loc = navigate(current_loc, user_input[1].capitalize())
            print('')
        elif command == "get" and len(user_input) > 1:
            if getitem(current_loc, inventory, user_input[1].capitalize()):
                rooms[current_loc]["item"] = None
            print('')
        else:
            print("Not a valid command.")
            print('')
        if current_loc == "Airlock":
            if len(inventory) == 6:
                print('You used the pistol to defeat the Alien! Congratulations on saving the ship and your crew!')
                print('Thank you for playing!')
                break
            else:
                print('Wow. That was easy.')
                print('The Alien was able to defeat you and your crew was left stranded.')
                print('Thank you for playing.')
                break

main()
