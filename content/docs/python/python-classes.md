---
title: "Python Classes"
description: ""
summary: "Create and maintain classes in Python."
date: 2024-10-29T22:48:58-07:00
lastmod: 2024-10-29T22:48:58-07:00
weight: 10
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

## Classes

Good Guide: https://realpython.com/python-classes/#getting-started-with-python-classes

### Basic Class

```python
class State:
  def __init__(self):
    self.current_menu = "main"
    self.choice = None
    self.user_data = {}  # Store any relevant user data
    self.some_var = function_call()

  def update_state(self, selection):
    self.choice = selection

state = State()
state.update_state(3)
```

### Class with state manager

```python
class State:
  def __init__(self):
    self.current_menu = "main"
    self.user_data = {}  # Store any relevant user data

  def update_state(self, selection):
    # Update state based on user selection
    # ...

  def get_next_action(self):
    # Determine next action based on current state
    # ...

class UI:
  def display_menu(self, menu_name):
    # Display menu options from state
    # ...

  def get_user_input(self):
    # Get user input and validate it
    # ...

class Logic:
  def perform_action1(self, data):
    # Business logic for action 1
    # ...

  def perform_action2(self, data):
    # Business logic for action 2
    # ...

class App:
  def __init__(self):
    self.state = State()
    self.ui = UI()
    self.logic = Logic()

  def run(self):
    while True:
      self.ui.display_menu(self.state.current_menu)
      user_input = self.ui.get_user_input()
      self.state.update_state(user_input)
      next_action = self.state.get_next_action()
      if next_action == "action1":
        self.logic.perform_action1(self.state.user_data)
      elif next_action == "action2":
        self.logic.perform_action2(self.state.user_data)
      # ... handle other actions ...
      elif next_action == "exit":
        break

if __name__ == "__main__":
  app = App()
  app.run()

```

### Check to see if attribute exists:

```python
def check_transmit(self):
  if not hasattr(self, "transmit"):
    print("Error message for you to see.")
    print("ERROR: Call setPipes() to define transmit address")
    raise ValueError("Transmit not set. See more information above.")
```
