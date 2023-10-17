# Project Details

- Project Name - Sprint 3: Mock
- Contributors - Connor Chen (cchen127) and Michael Tu (mstu)
- Total Est. Time - 20 hours (6 hours of which was paired programming)
- Repo link: [https://github.com/cs0320-f23/mock-cchen127-mstu.git](https://github.com/cs0320-f23/mock-cchen127-mstu.git)

# Design Choices

For our implementation of Mock, the vast majority of our program is handled in the REPLInput.tsx class. The App.tsx serves as our project's highest level component, which consists of the REPL.tsx class which updates based on user input. The REPLHistory.tsx class handles the history of submitted commands and aids in outputting and showing the results and history of user inputs, while the ControlledInput.tsx serves as an input box that wraps the REPLInput to use React to manage state. However, as these classes were all inspired by the Mock gearup/livecode, we will go more in depth on our design choices for the REPLInput class.

## General design choices/structure for REPLInput

Let's first take a look at the main components of the REPLInput class:

### REPLInputProps

The REPLInputProps contains three components of state that we incorporated and update throughout the execution of our program. These include outputHistory/setOutputHistory (which is an array of JSX.Elements and is updated whenever a command is submitted), mode/setMode which lets the program know whether the app is in brief mode or verbose mode as a boolean, where brief is true and verbose is false.

### toHtmlTable helper function

Next, we have a helper function that helps us create HTML tables given a double array of data. This function is used for viewing and searching a file (more on this later).

### REPLInput function

Our REPLInput function creates the REPLInput component (which is a text box that commands are inputted by the user) based on given properties. We initially define two constants called commandString and displayMode; the former of which is used for managing the contents of the input box, whereas the latter is used for displaying the current mode of the app to the user in the button text (this is stored and updated as a string for ease of implementation). We then prepare mock data maps to run/test our front end application. These include mocked filepaths that correspond to mocked parsed CSV data, as well as mocked Json responseMaps to handle our eventual back end's responseMaps.

And within the REPLInput function, we have... (to be completed after integration in the next sprint)

## Choices for handleSubmit function

The handleSubmit function in REPLInput is the main part of our program, handling user-inputted commands. The function starts out by parsing the entered string into an array of strings, by utilizing a regex as well as splitting the string into an array.

Let's go through each of the four commands that correspond to each of the first four user stories:

### Mode switching

As outlined in the handout for user story 1, the app starts by default in brief mode, which only shows the end output for a submitted command. Since the commandString is parsed into a commandArray, for all our command handling we check the first element in the commandArray to determine which command the user wishes to execute. To switch to verbose mode or switch back into brief mode, we use a series of if statements, which end up setting the props mode to true or false, changing the displayMode to either "brief" or "verbose", and add an appropriate JSX.Element to the output history for outputting in the history on the application.

### Loading files

In terms of loading files, we implemented very similar logic with if statements first determining whether the current state was in brief or verbose mode. The provided filepath in this scenario must be one of the specified mock filepaths that were defined in one of the maps. Other than that, the logic is rather simple where the user will see the command history output based on the specified mode.

### Viewing files

Viewing files once again continues the similar logic, however this time we also add an extra condition to ensure that there is a file loaded (through checking the length of the props.data array and ensuring that it is greater than 1). If there is no data loaded, the app spits out an error message; if there _is_ data, the app will utilize the aforementioned toHTMLTable function to convert the 2d array into an HTML table to be outputted.

### Searching on files

Searching files continues the conditional structure. The conditionals first check the type of the second string (`column` operator) in the input command. If it's a `number`, searching by column index will be used on the backend. If it's a string, searching by column name will be used on the backend. Errors will be produced if a file has not been loaded or if the command is in the wrong format. A successful response will output the search results in an HTML table. Searching for multi-word values/column names is handled in splitting the command input at the top of `handleSubmit()`.

### Invalid command handling

For all of the commands above, we've considered scenarios where a command is inputted improperly, and handled them as such by outputting error messages to the user in relative detail about what the issue is. We also incorporated an error message if the user submits an input that is not a command at all, providing the user feedback if there was a typo and not just returning nothing. This error output also changes based on the current mode.

### End result (what is returned)

The REPLInput function returns a JSX.Element which incorporates all of the components mentioned above, ending with the handleSubmit function. There is the submit button that also dynamically changes its display, reflecting any changes in the mode (which was very helpful for debugging and testing purposes). We also decided to include a quality of life change by allowing the user to simply hit the enter key to submit a command in the input bar.

this then leads into the ... (to be completed after integration in the next sprint)

## REPL.tsx

Since the App is the highest level class in this program, the REPL.tsx file manages both the REPLHistory and REPLInput in order to finally hand it off to the App.tsx file.

# Runtime

Runtime was not optimized as it was not part of the user stories, but we used mocking in order to ensure the functionality of our program without having to necessarily wait for back end integration and potential runtime there utilizing actual data and processing.

# Errors/Bugs

We did not run into any major errors or bugs during the development of this project.

In handling `search` commands, if the headers are named as integers, the current search handling will bug and handle it as a search by column index, instead of a search by column name.

# Tests

Our testing suite focused mainly on integration testing with Playwright. We wrote numerous tests for each functionality of our program, including interactions between the programs.

One thing that came up when figuring out how to test was that the Playwright documentation encouraged us to utilize the getByRole for locating dynamic elements (like buttons, input bars, tables) whereas we should utilize text locators for locating static elements (search results). We decided to use this methodology in our testing.

## Mock app tests (general app elements/testing)

The mock app set of tests simply test that our app and webpage load correctly and display what we intend to display. We tested to make sure the button loads, the input bar loads, as well as entering invalid/empty inputs into the input bar, in both brief and verbose mode as well as with clicking the button and hitting the enter key.

## Mode tests

For our mode tests, we first started out by ensuring that the default mode was in brief. We then tested interactions between switching to verbose and switching from brief to verbose back to brief. These were also the last set of tests where we explicitly tested all combinations of either clicking the submit button or hitting the enter key as we figured it was redundant at this point seeing as we were confident that either submit method was valid. We also tested situations where the mode command is inputted but the provided mode to switch to is not valid (trying to switch to a mode other than brief or verbose).

Although we did not specify below, we did also test interactions with mode switching for each of the other command tests.

## Load tests

For our load tests, we first tested successfully loading and failing to load files (with invalid filepaths), expecting success and/or error messages. We then tested the compatibility of loading a file and then loading another (we tested to make sure the data actually updated by viewing in the later integration test).

## Integration tests (tests interactions between load, view, and search commands)

For integration tests, we first started out with loading and viewing files. We then tested viewing without loading a file (which returns an error message), as well as finally testing that interaction between loading a file and viewing it then loading another file then viewing it to ensure the data changes.

Integration tests involving search include: searching in two different CSVs (load, search, load, search), switching modes and searching (mode, search, mode, search), and the most basic user workflow (load, view, search).

## Search tests

The search tests test the correct output (success or error) for each block of the giant conditional statement in `REPLInput.tsx`. Remaining search tests are handled in `integration-tests.spec.ts`.

# How to

To start the server, the user must first enter "cd mock" into a terminal and navigate towards the provided url for the localhost webpage. The user can then enter desired inputs into the search bar, and submitting through either the button at the bottom or simply hitting the enter key. The button also displays which mode the app is currently in.

To run the tests, a simple way would be to install a Playwright extension in VSCode to run tests easily by clicking the green arrow next to each test. However, a developer can also run tests by typing "npx playwright test --ui" into a terminal; _one must ensure the server has been started (either in a separate terminal or another method) before running tests or else Playwright will not be able to access the desired webpage in the testing suite_.
