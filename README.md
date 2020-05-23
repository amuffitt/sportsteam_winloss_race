# Summary
This project is a "program" I developed that displays comparison charts of sports team win/loss records over time across custom year ranges based on dynamic input values.  Currently, it encompasses NFL, NBA, MLB, and NCAA Football for the years 1960-2019.

There are 2 versions:

1) **sports_winloss_race**: This can be accessed by anyone at https://amuffitt.github.io/sportsteam_winloss_race/sportsteam_winloss_race.html and uses the Binder service to host the notebook, convert it to an HTML page that works with dynamic content, and power the selector widgets.  This version displays the final resuts on submission, but does not show any year by year race animations due to various JupyterLab javascript restrictions.

2) **sports_winloss_race_animation**: This is the "full feature" program but can currently only be used by anyone that is able to open and run a Jupyter notebook (likely users familiar with Python).  It adds an option (enabled by default) to display the result race chart as a year by year animation over time (as well as an option to just display the final result like the above version).  

A few of the main technologies/features learned and used in this effort:

- **ipywidgets**: interactive HTML widgets for Jupyter notebooks used to allow users to easily select input values in a "UI" format versus making them edit cells in a notebook
- **NBInteract**: Python package that provides a command-line tool to generate interactive web pages from Jupyter notebooks. It allows Jupyter widgets to remain interactive even when the notebook is converted to static HTML by using Binder servers as the computational backend.  This made it possible to host a Github Pages webpage with the program versus only having it available in a separate Jupyter Notebook
- **MyBinder** service:  Part of using NBInteract, Binder basically copies and hosts a github repository, builds a Docker image out of it based on specified dependencies, and hosts content on a JupyterHub server
- **Matplotlib Funcanimation**: animation feature that plays an 'slidshow' of the year by year team race
- **Pandas Cumulative Sum** (cumsum): Used to maintain a running sum of the career stats for each team over multiple years


# Background

As a sports fan, I wanted to develop a way to quickly compare records for any team across any year range for football, basketball, and baseball.  The results would either be displayed in a final chart or as a year by year animation to show teams losing and gaining ground on one another depending on how well they did each year.    Once this was done as in initial step by altering lists and variables in a Jupyter Notebook, I wanted to take it a step further by creating a user interface for a user to easily select year and team inputs to generate the desired output.  Finally, once THAT was done, I wanted to make the "program" available to people to run outside of Jupyter Notebook just by going to a URL (besides nbviewer which doesn't work with dynamic widgets) to enable even those not familiar with Jupyter notebooks to run the funcionality.

# Why 2 versions?

My original vision of the project was what's in the sports_winloss_race_animation notebook, allowing the user to view year by year animations of the race between teams as well as displaying the final result.  However, after utilizing NBInteract and Binder to make the notebook an interactive publicly available UI/webpage to make selections and generate results, I ran into numerous problems with getting the animation to work in JupyterLab (which is what NBInteract uses) due to their heavy restrictions on javascript/interactive content (to prevent potential malicious code).  Quick summary of problems:

- Using %matplotlib inline : There's an option to display a funcanimation that works with %matplotlib inline using "to_jstml()" which creates a player frame to play the content.  The problem is that it is blocked when launched on user initiation (versus run automatically) which is needed here since the user has to make input selections. The message shown is  "JupyterLab does not execute inline JavaScript in HTML" and is a well known limitation/challenge with JupyterLab.  Also tried having the user intiation run the next cell and having the to_jshtml() launch automatically there, but the javascript code to move to the next cell is also blocked.

- Using %matplotlib notebook: Doesn't appear that %matplotlib notebook works in Jupyterlab at all given it's interactive nature.  Running anything with %matplotlib notebook shows the message "JavaScript output is disabled in JupyterLab"

- Using %matplotlib widget: This is similar to %matplotlib notebook and uses the ipympl functionality that can make it possible to run interactive content.  However, on  JupyterLab the animation doesn't display and just the text "Canvas(toolbar=Toolbar(toolitems= [(‘Home’, ‘Reset original view’, ‘home’, ‘home’)"    The Binder server running the content has all the correct extension versions etc noted to run %matplotlib widget, but for some reason there's some conflicts/issues (Dev Tools shows "Could Not Instantiate Widget".  . Despite a lot of research, no solutions for this have been found and the remote nature of the Binder servers makes it hard to do too much debugging

So, ultimately I split it up into 2 versions to at least make some of it useable from a URL and the full featured version for now will only be accessible in Jupyter Notebooks until a solution to the JupyterLab animation issue is found.  


# Uploaded Files

- **Jupyter Notebook (no animation) - sports_winloss_race.ipynb**  .  This version displays the final race result without the animation option.  One main difference is it uses the %matplotlib inline backend which works fine with Jupyterlab and the related Github Pages content but can only display static content (no animation)
- **Related HTML file generated by NBInteract and hosted on Binder  - sports_winloss_race.html**
- **Jupyter Notebaook (with animation)** - sports_winloss_race_animation.ipynb   This version has options for both year by year race animation and just showing the final result.  It uses the %matplotlib notebook backend which enables interactive content like animations.
- **Sports Records by Year** - Excel file read in Python containing the following columns: Year, Team, Wins, Losses, Ties, Sport, Conf  (code in the notebook manipulates the dataframe to add additional columns for totalwins, totallosses, and totalties using cumulative sum to track records over time
- **Binder/NBInteract files**  - The first several rows listed in the repository files are automatically generated from the Binder integration and requirements.txt specified what Python libraries to install in Binder (like Matplotlib etc)


