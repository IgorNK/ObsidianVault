```
// Using "##" to display same title but have unique identifier.
ImGui::Begin("Same title as another window##1");
ImGui::Text("This is window 1.\nMy title is the same as window 2, but my identifier is unique.");
ImGui::End();

ImGui::Begin("Same title as another window##2");
ImGui::Text("This is window 2.\nMy title is the same as window 1, but my identifier is unique.");
ImGui::End();

// Using "###" to display a changing title but keep a static identifier "MyWindow"
char buf[128];
ImFormatString(buf, IM_ARRAYSIZE(buf), "Animated title %c %d###MyWindow", "|/-\\"[(int)(ImGui::GetTime()/0.25f)&3], rand());
ImGui::Begin(buf);
ImGui::Text("This window has a changing title.");
ImGui::End();
```

![[Pasted image 20231216095134.png]]

```
A primer on the meaning and use of ID in ImGui:

   - widgets require state to be carried over multiple frames (most typically ImGui often needs to remember what is the "active" widget).
     to do so they need an unique ID. unique ID are typically derived from a string label, an integer index or a pointer.

       Button("OK");        // Label = "OK",     ID = hash of "OK"
       Button("Cancel");    // Label = "Cancel", ID = hash of "Cancel"

   - ID are uniquely scoped within Windows so no conflict can happen if you have two buttons called "OK" in two different Windows.

   - when passing a label you can optionally specify extra unique ID information within string itself. This helps solving the simpler collision cases.
     use "##" to pass a complement to the ID that won't be visible to the end-user:

       Button("Play##0");   // Label = "Play",   ID = hash of "Play##0"
       Button("Play##1");   // Label = "Play",   ID = hash of "Play##1" (different from above)

     use "###" to pass a label that isn't part of ID. You can use that to change labels while preserving a constant ID.

       Button("Hello###ID"; // Label = "Hello",  ID = hash of "ID"
       Button("World###ID"; // Label = "World",  ID = hash of "ID" (same as above)

   - use PushID() / PopID() to create scopes and avoid ID conflicts within the same Window:

       Button("Click");     // Label = "Click",  ID = hash of "Click"
       PushID("node");
       Button("Click");     // Label = "Click",  ID = hash of "node" and "Click"
       for (int i = 0; i < 100; i++)
       {
          PushID(i);
          Button("Click");  // Label = "Click",  ID = hash of "node" and i and "label"
          PopID();
       }
       PopID();
       PushID(my_ptr);
       Button("Click");     // Label = "Click",  ID = hash of ptr and "Click"
       PopID();

     so if you have a loop creating multiple items, you can use PushID() / PopID() with the index of each item, or their pointer, etc.
     some functions like TreeNode() implicitly creates a scope for you by calling PushID().

   - when working with trees, ID are used to preserve the opened/closed state of tree nodes.
     depending on your use cases you may want to use strings, indices or pointers as ID. 
      e.g. when displaying a single object that may change over time (1-1 relationship), using a static string as ID will preserve your node open/closed state when the targeted object change.
      e.g. when displaying a list of objects, using indices or pointers as ID will preserve the node open/closed state differently. experiment and see what makes more sense!
```

#gamedev #imgui