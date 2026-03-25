# Making a Quarto dashboard

This demo takes an initial script, `01_water_quality_viz.R`, and adapts it into a Quarto dashboard, `index.qmd`.

- A simpler version of the dashboard, `02_initial_dashboard.qmd`, is included to demonstrate how one might start turning the script into a dashboard, while `index.qmd` is the result of more modifications and additions.
- The Quarto dashboard file is called `index.qmd` so that the rendered document is `index.html`, which GitHub Pages will use to make a webpage. More details below.

To start, look at the initial script, `01_water_quality_viz.R`.

- First, the script reads in a comma-separated values (csv) file of water quality data, reads in stream gauge data from USGS using the `dataRetrieval` package, and renames the variables.
- After that, the script is broken into three sections with section headings:
    - monthly summary statistics
        - compute some summary statistics about when temperature, dissolved oxygen (DO) and pH exceed certain threshholds
        - make a table
    - plot each variable in its own subfigure
        - join the two data sets (data from the water quality csv file and data from dataRetrieval)
        - pivot the combined dataset for plotting
        - make the parameter column a factor variable
        - rename the levels (parameter names) for the plots
        - plot each variable in its own facet/panel
    - plot each variable, stacked so that dates align
        - make the plot
        - save it to file
     
