disambiguation: If you're looking for `bpy.data.texts` _text datablocks_ go [here](bpy_data_texts)

### Text Objects / Font Objects

In Blender _Text Objects_ are `type == 'FONT'`, and are closely related to the Curve data type. The big difference is that the individual glyphs can't be edited. You must convert to `type == 'CURVE'` first. Text Objects have many parameters. The most important parameter of the `'FONT'` type is possibly the font face / family (like Helvetica,..Gill Sans). Confusingly this property is called `.font`.

```python

import bpy

def make_text_object(name, txt, props):
    scene = bpy.context.scene
    curves = bpy.data.curves
    objects = bpy.data.objects

    name = props.get(name, 'default name')

    # CURVES
    if not (name in curves):
        f = curves.new(name, 'FONT')
    else:
        f = curves[name]

    # CONTAINER OBJECTS
    if name in objects:
        obj = objects[name]
    else:
        obj = objects.new(name, f)
        scene.objects.link(obj)
    
    # there's two checks, 1) did we pass a font name, 2) is it valid.
    default = bpy.data.fonts.get('Bfont')
    if props.get('fontname'):
        f.font = bpy.data.fonts.get(props['fontname'], default)
    else:
        f.font = default

    f.body = txt

    setters = [
        'size',
        # space
        'space_character',
        'space_word',
        'space_line',
        'offset_x',
        'offset_y',
        # modifications
        'offset',
        'extrude',
        # bevel
        'bevel_depth',
        'bevel_resolution',
        'align'
    ]
    
    # some dynamic programming
    for propname in setters:
        if props.get(propname):
            setattr(f, propname, props.get(propname))

    return obj

make_text_object('my_text_object', 'far out man', {})
```