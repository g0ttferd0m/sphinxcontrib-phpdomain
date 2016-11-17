# sphinxcontrib-phpdomain

##Fixing little bugs...

I use Sphinx on OpenSuse Leap 42.1 with python 3.4.

I wanted to create documentation for PHP project, so I wanted to use __sphinxcontrib-phpdomain__ BUT,

to much bugs, firstly, this version of phpdomain use iteritems() which is not used by Python 3 anymore.

So thanks to StackOverflow I saw that items() must replace iteritems(). So do I. 

After that, another pb, in the function clear_doc() there was a pb in the loop because of size change of dictionnary.

So with some friends help we try some solutions and the best one was to import copy and use copy.deepcopy.

Here the sample code before and after modifications:

```python
def clear_doc(self, docname):
        for fullname, (fn, _) in self.data['objects'].iteritems():
            if fn == docname:
                del self.data['objects'][fullname]
        for ns, (fn, _, _) in self.data['namespaces'].iteritems():
            if fn == docname:
                del self.data['namespaces'][ns]
```

and 

```python
def clear_doc(self, docname):
        for fullname, (fn, _) in copy.deepcopy(self.data['objects']).items():
            if fn == docname:
                del self.data['objects'][fullname]
        for ns, (fn, _, _) in copy.deepcopy(self.data['namespaces']).items():
            if fn == docname:
                del self.data['namespaces'][ns]
```

I let here the backtrace of the bugs : 

```bash
Exception occurred:
  File "/usr/lib/python3.4/site-packages/sphinxcontrib/phpdomain.py", line 506, in generate
    content = sorted(content.iteritems())
AttributeError: 'dict' object has no attribute 'iteritems'

```bash
Exception occurred:
  File "/usr/lib/python3.4/site-packages/sphinxcontrib/phpdomain.py", line 564, in clear_doc
    for fullname, (fn, _) in self.data['objects'].items():
RuntimeError: dictionary changed size during iteration

