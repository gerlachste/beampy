.. _changelog:

Change-log
==========

0.5.4
-----

Major version update, Beampy is now compatible with python 3 (tested on 3.7)

* Python 3 compatible 
* Fix multi format output duplication of texts `(issue #13) <https://github.com/hchauvet/beampy/issues/13>`_.
* Introduce integrative tests using pytest framework (still no unit-tests for the core functions)
* Improve logging.debug outputs
* Add option to specify the location of the presentation source file ( doc = document(source_filename=__name__) )

0.5.3
-----

Some minor fix:

* Fix bug in layer propagation for box
* Fix bug when last layer is set as [n:]
* Start using logging.debug in modules

0.5.2
-----

* Fix bug with dvisvgm output for tikz dvi (function latex2svg has now
  an option to write the svg produced by dvisvgm).
* Add tableofcontents modules `See documentation <auto_examples/plot_TOC.html>`_.
* Add a BeamerFrankfurt theme `See documentation <auto_themes/theme_BeamerFrankfurt.html>`_.
* Figure module accepts animated gif.
* Extra latex packages could be added to text module with the
  "extra_packages" argument.
* Compute svg rectangle and circle size which improve the rendering
  speed (no need to call Inkscape).
* Improve box module (it is now a subclass of group) `See documentation <auto_examples/plot_box.html>`_.
* Correct some scale factors in the convert_unit function.
* Width and height are now Length objects and accept complex
  operations like:
  
  .. code-block:: python

     # 50% of the currentwidth 
     a = rectangle(width='50%', height=10)

     # width/height relative to the a element
     b = rectangle(width=a.width/2+'2cm', height=a.height/'10pt')


* module position (x,y) operation now accept Length objects (width/height):

  .. code-block:: python

     a = rectangle(width='50%', height=10)

     b = rectangle(x=a.width+'2cm', y=a.height+5)

  
* Change the core of beampy to render elements when needed for
  operation on position or length (i.e. when you make an operation on
  an element width or height that is unknown, the element will be
  rendered to get its size and allow the operation)
* Add "zorder" operation for modules (above/below/last/first) to change
  their overlay order:

  .. code-block:: python

     a = rectangle(x='center', y='center', width=50, height=50)
     b = rectangle(x='center', y='center', width=a.height+100,
                   height=a.height+100, color='red')

     # Make b appears below a
     b.below(a)
  
     # equivalent to a.above(b) or a.last() or b.first()


0.5.1
-----

* Fix several bugs in bokeh figures (thx to `Silmathoron <https://github.com/Silmathoron>`_)
* Fix bokeh figure resizing, it's now use the "sizing_mode = scale_both" from bokeh and revert the css transform scaling for the bokeh div.
* Fix javascript loader for bokeh (new bokeh version (>0.12.6) named their main div "bk_root")
* Add function to cache file in beampy cache class
* Cache javascript external libraries files from bokeh (download from their CDN, if "doc = document(cache=False)")
  
0.5.0
-----

* Add box function to decorate group
* New experimental way to write text inside presentation using context manager

  .. code-block:: python
		  
     with text(width=400):
         """
	 Any comment inside the context manager will be passed to the
	 text function as input argument. This allows clearer source
	 when writing long texts.

	 No more need to add an *r* before to protect the text passed to
	 latex, it's now automatically added.
	 """
	 
* Correct bug when only html object are present in one slide
* Correct small typos in the install section of the documentation.

0.4.9
-----

* Add documentation in Beampy module
* Add 'anchor' key to position dictionary to define anchor along the
  bounding-box of module to place them.
* Add utils.py to store functions that call beampy modules. 
* Add function **bounding_box(module)** to utils.py to draw bounding box with
  anchors around Beampy modules. Add also a function to draw axes on slide,
  **draw_axes()**

0.4.8
-----

* Partially fix issue #12.
* Clean code syntaxe. 

0.4.7
-----

* Introduce layer mechanism. Slide elements can be animated by layers allowing mechanism like beamer "\only".
  The layer are managed as python slicing on Beampy modules.

  .. code-block:: python

     with slide('Test layers'):
         text('First printed on layer 0')
         text('Secondly printed on layer 1')[1]
         text('Printed from layer 2 to 3')[2,3]
         text('Printed on all layers')[:]
         text('Printed on layer 4')[4]
    
     with group(width=300)[2:]:
         text('Printed inside group')
         text('for layers 2 to end')

0.4.6
-----

* The core of Beampy slide processor has been rewritten and now allows recursive group of elements.

  .. code-block:: python

     with group():
        text('toto')
        with group(width=300):
            text('tata')

     with group(width=200):
        figure('./niceplot.pdf')
        text('nice legend')


* If a group width is given all elements in groups without specified width take the width of the group

  .. code-block:: python

     with group(width=200):
         figure('./niceplot.pdf')
         text('nice legend')
         # Figure and text width will be automatically set to 200 px


* Relative placement now could be done on auto positioned elements

  .. code-block:: python

     t0 = text('toto')
     text('tata', x=t0.center + center(0), t0.bottom + 0.1)


* Video now could use external links (with *embedded=True*) rather than be included in the html file.
  The video is loaded from disk (be careful with file path) when the slide is displayed on screen.

0.4.5
-----

* All texts are preprocessed in a single latex file (Latex is called only once: improve compilation time)
* Cache bug fix: Video and svg are now cached correctly

0.4.4
-----

* Improve cache: one file per element cached (don't write the cache twice!)
* Svg: Add line and rectangle commands to easily draw lines and rectangles
* Relative placement: add shortcut center(shift), right(shift) and bottom(shift) 
  to change the anchor of the current element.
  
  .. code-block:: python

    e1 = text('Somthing', x=0.2, y=0.4)
    e2 = text('An other thing', 
              x=e1.left + right(0.1), 
              y=e1.center + center(0))



0.4.3
-----

* Matplotlib figures can now be directly passed to *figure()* or a list of matplotlib figures can be animated with *animatesvg()*
* Minor improvement in cache size (content is no more stored in cache file)
* Update of scour version (svg-optimiser) 

0.4.2
-----

* Glyph paths from Latex are now unique (this reduce the number of svg lines in documents)
* Add *svg* command ton include raw svg in slide
* Improve the theme flexibility, a background with interactive elements can now be created!

0.4.1
-----

* All slide are now loaded into ram, improve speed
* Modules are now classes which inherit from a base class "beampy_module" in modules/core.py
* cache is now unique for all format (pdf, svg, html) and special keys can be added
  to modules in order to create their chache id


