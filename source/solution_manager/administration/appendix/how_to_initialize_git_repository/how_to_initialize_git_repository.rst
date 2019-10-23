==================================
How To Initialize a Git Repository
==================================

.. TODO: In Denodo 8.0, remove this section.

Starting with the update 7.0 20190903, the Solution Manager automatically initializes the Git repository used, if it has
not been initialized yet.

With previous updates of the Solution Manager, you have to manually initialize the Git repository before 
using it. Otherwise, the Solution Manager will return an error when trying to store information there.

To initialize a git repository, open a command line and execute this from your home directory:

.. code-block:: bash
   :emphasize-lines: 4
   
   mkdir new_repository
   cd new_repository
   git init
   git remote add origin <URL of the Git repository>
   echo README file > README.md
   git add -A
   git commit -m "Initial commit"
   git push -u origin master
   cd ..

In line #4, replace "<URL of the Git repository>" with the URL of the repository.

After running this, the repository will be initialized.

You can now delete the directory ``new_repository``. 