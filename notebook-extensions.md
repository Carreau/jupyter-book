# Notebook extensions

Notebook extensions, or plugins allow end user to highly control the behavior and the appeareance of the Notebook application.

Extensions capability can highly vary from beeing able to load notebook files from [Google Drive](https://github.com/jupyter/jupyter-drive), or PostGresSql server, Presenting Notebook in the form of Slideshow, or by just adding a convenient button or keyboard shortcut for an action the user is often doing.

The way we write Jupyter/IPython is to provide the minimal sensible default, with easy access to configuration for extension to modify behavior.


Extensions can be composed of many pieces, but you will mostly find a Javascript part that live on the frontend side (ie, the Browser, written in Javascript), and a part that live on the server side (written in Python). We will, in a first time mostly focus on the Javascript side.

A large number of repo exist here and there o nthe internet, and we haven't taken the time to write a Jupyter Store (yet), to make extension easily installable. Well I suppose this can be done as an extension, and your research on the web will probably show that it can be done, but we will still focus on the old manual way of installing extension to learn how things works, because that's why you are here right ?

Ok, so here are link to some active repos, with extensions:

 - https://github.com/ipython-contrib/IPython-notebook-extensions – check out the rigth branch dependsing on your version of IPython. If you are using IPython 3.x you most likely want extension from the 3.x branch.
 - https://bitbucket.org/ipre/calico/ look in the notebook/nbextesion folder.

 
