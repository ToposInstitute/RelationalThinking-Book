# Guide to build the book

This repo contains files to build a Jupyter Book with Thebe live code execution for code in Algebraic Julia. The book has been published online via Github Pages.

To add content and build this book, refer to the steps below: 

1. Install Jupyter Book in your local machine - https://jupyterbook.org/en/stable/start/overview.html

2. Clone this repo to a local folder

3. Add new markdown files - https://jupyterbook.org/en/stable/start/new-file.html 

4. Rebuild the book using the command 
    ```
    jupyter-book build mybookname 
    ```
    The above command will provide a local link to view the book. `git push` once the changes are satsifactory. 

5. To publish your changes to the online book hosted at the Github Pages, use `ghp-import` - https://jupyterbook.org/en/stable/start/publish.html

    - Install `ghp-import` using the command 

    ```
    pip install ghp-import
    ```

    - To publish changes to the Github Pages, from the `main` branch execute 
    ```
    ghp-import -n -p -f _build/html
    ```
        
