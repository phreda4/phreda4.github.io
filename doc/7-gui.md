# Graphics User Interface

## Immediate Mode User Interface

After several versions of building a gui for :r4 I found the one type of GUI called IMGUI, this fits perfectly with the concept of forth. The idea here is to draw the interface while the interaction is calculated, that is, there is no way to save but the behavior is being executed while drawing.

The implentatios is simple, every gui element have an id, in the case of button, if the mouse enter in this element, the id is store and if the mouse unclick this id, the vector of execution is called.

The zones are managed with the gc library: xc,yc and w,h shared by sprites and others.



