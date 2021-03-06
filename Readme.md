[![Coveralls – test coverage
](https://img.shields.io/coveralls/studio-b12/doxie.inject.svg?style=flat-square)
](https://coveralls.io/r/studio-b12/doxie.inject)
 [![Travis – build status
](https://img.shields.io/travis/studio-b12/doxie.inject/master.svg?style=flat-square)
](https://travis-ci.org/studio-b12/doxie.inject)
 [![David – status of dependencies
](https://img.shields.io/david/studio-b12/doxie.inject.svg?style=flat-square)
](https://david-dm.org/studio-b12/doxie.inject)
 [![Stability: unstable
](https://img.shields.io/badge/stability-unstable-yellowgreen.svg?style=flat-square)
](https://github.com/studio-b12/doxie.inject/milestones/1.0)
 [![Code style: airbnb
](https://img.shields.io/badge/code%20style-airbnb-blue.svg?style=flat-square)
](https://github.com/airbnb/javascript)




<h1                                                                 id="/"><pre>
doxie --inject
</pre></h1>


A plugin for *[doxie][]*.  
**Join rendered docs and inject them into your readme.**

[doxie]:  https://github.com/studio-b12/doxie




<p align="center"><a
  title="Graphic by the great Justin Mezzell"
  href="http://justinmezzell.tumblr.com/post/95370140878"
  >
  <br/>
  <br/>
  <img
    src="Readme/Scissors.gif"
    width="400"
    height="300"
  />
  <br/>
  <br/>
</a></p>




Installation
------------

`doxie --inject` is a plugin for the command-line tool *[doxie][]*. *[dox][]* produces compatible data from jsDoc comments. Install all three if you haven’t already:

```sh
$ npm install --global dox doxie doxie.inject
```

[dox]:               http://npm.im/dox




Usage
-----

<a                                               id="/usage/write-a-readme"></a>
1) **Write a readme**

…or some other document in Markdown or HTML. Put the markers `<!-- @doxie.inject start -->` and `<!-- @doxie.inject end -->` somewhere in it.


2) **Profit!**

Render your docs with `doxie` – here we use [`doxie --render`] for that. Then `--inject` them into your readme.

```sh
$ dox | doxie --render --inject
```

We’ll join your docs into one string and replace all content between the [markers][] with that string.

[`doxie --render`]:  http://npm.im/doxie.render
[markers]:           #/usage/write-a-readme


3) **Options!**

You can set things up with options:

    --inject( <option> <argument>)*

For example:

    $ doxie --render \
    $   --inject into 'My docs.md' as public

Read on!




Options
-------


&nbsp;


<h3                                                               id="/as"><pre>
--inject as &lt;marker name&gt;
</pre></h3>

Apart from the [default markers][markers] you can have named markers. This way you can inject different docs at different places.

Put them in your readme: `<!-- @doxie.inject start my-marker -->` and `<!-- @doxie.inject end my-marker -->`.

Then inject your docs:

    $ doxie --inject as my-marker


&nbsp;


<h3                                                             id="/into"><pre>
--inject into &lt;target document&gt;
</pre></h3>

By default we’ll inject your docs into `README.md`, `Readme.md`, or `readme.md` in the current working directory. But if you want another target, no problem:

    $ doxie --inject into ./documentation/my-docs.html


&nbsp;




Programmatic usage
------------------

You can use `doxie.inject` directly with *[doxie-core][]*. Install both if you haven’t already:

```sh
$ npm install doxie-core doxie.inject
```

[doxie-core]:        http://npm.im/doxie-core


Use it like this:


&nbsp;


<h3                                                           id="/inject"><pre>
inject({input, [as]})
  → plugin
</pre></h3>

**Parameters** (properties of an object):

* **`input`**  
  <sup>type: `String` (or cast to `String`) | required</sup>  
  The content of a readme – or another Markdown / HTML document.

* **`as`**  
  <sup>type: `String` or `null` (or cast to `String`) | optional | default: `null`</sup>  
  Same as `--inject as <marker name>`.


**Returned value:**

* **[`plugin`](#/plugin)**  
  <sup>type: `Function`</sup>  
  Pass this to *[doxie-core][]*.


&nbsp;


<h3                                                           id="/plugin"><pre>
plugin(…)
  → {['doxie.inject']: {output}, …}
</pre></h3>

**Returned properties:**

* **`['doxie.inject'].output`**  
  <sup>type: `String`</sup>  
  The resulting content of the readme – with your docs injected.

* **`…`**  
  Other properties required by *[doxie-core][]*.


**Example:**

```js
const doxie = require('doxie-core');
const render = require('doxie.render');
const inject = require('doxie.inject');

doxie([
  render(require('./.doxie.render.js')),
  inject({
    input:
`#  My readme  #

##  Usage  ##
<!-- @doxie.input start usage -->
(this will be replaced)
<!-- @doxie.input end usage -->

##  License: MIT  ##
`,
    as: 'usage',
  }),
])([/* my docs’ data */]);
//» {
//    'doxie.inject': {output: "
//      #  My readme  #
//      
//      ##  Usage  ##
//      <!-- @doxie.input start usage -->
//      
//      ###  myFancyFunction(a, b, c)  ###
//      
//      Parameters:
//      
//      * a
//      * b
//      * c
//      
//      Return value:
//      
//      * d
//      <!-- @doxie.input end usage -->
//      
//      ##  License: MIT  ##
//    "},
//    …
//  }

```


&nbsp;




License
-------

[MIT][] © [Studio B12 GmbH][]

[MIT]:              ./License.md
[Studio B12 GmbH]:  http://studio-b12.de
