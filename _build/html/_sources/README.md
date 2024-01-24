# Guide to add contents and build the book

This repo contains files to build a Jupyter Book with Thebe live code execution for code in Algebraic Julia. The book has been published online via Github Pages at https://toposinstitute.github.io/AlgJulia-Book/intro.html.

Clone this repo into a local folder. 

## Build the book

1. Install Jupyter Book in your local machine - https://jupyterbook.org/en/stable/start/overview.html

2. To build the book, navigate to the folder of the book and run
    ```
    jupyter-book build .
    ```
    The above command will provide a local link to view the book.
    
    Push the changes to the repository using `git push` 


## Adding content to the book : adding new chapters, modifying existing chapters

Refer to https://jupyterbook.org/en/stable/start/new-file.html 


## Publishing the book to GitHub pages

To publish your changes to the online book hosted at the Github Pages, use `ghp-import` - https://jupyterbook.org/en/stable/start/publish.html

    - Install `ghp-import` using the command 

    ```
    pip install ghp-import
    ```

    - To publish changes to the Github Pages, from the `main` branch execute 
    ```
    ghp-import -n -p -f _build/html
    ```

        
