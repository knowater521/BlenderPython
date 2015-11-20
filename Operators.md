Most of the useful information about Operators can be acquired by taking your time to read through the add-on list. It's how I learnt it. My favorite snippets i've worked into a [sublime text editor autocomplete snippet set](https://github.com/zeffii/BlenderSublimeSnippets). The [bpy docs also cover](http://www.blender.org/api/blender_python_api_current/info_quickstart.html?highlight=operator) the various forms of `Operators` in some detail.  
  
My favorite things about the API might be worth explicitly mentioning. 

### Registration

If you have an addon / script with one class, you might register/unregister it using. Panels, Operators..etc.
```python
def register():
    bpy.utils.register_class(YourClassName)

def unregister():
    bpy.utils.unregister_class(YourClassName)
```

As your add-on becomes more complex you'll want to register more than 1 Class, but you probably don't want to be explicit about the class name for each new class. This registers all `bpy` classes in the order that they appear in the file.

```python
def register():
    bpy.utils.register_module(__name__)

def unregister():
    bpy.utils.unregister_module(__name__)
```

that's instead of writing:

```python
def register():
    bpy.utils.register_class(ClassNameOne)
    bpy.utils.register_class(ClassNameTwo)
    bpy.utils.register_class(ClassNameThree)

def unregister():
    bpy.utils.unregister_class(ClassNameOne)
    bpy.utils.unregister_class(ClassNameTwo)
    bpy.utils.unregister_class(ClassNameThree)

```