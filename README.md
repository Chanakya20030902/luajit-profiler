A luajit profiler that builds an interactive html page. It combines the output of jit.attach and jit.profile to give you a sense of how well or not well your luajit application compiles. 

checkout a demo: https://capsadmin.github.io/luajit-profiler (this is from running the test suite in [NattLua](https://github.com/CapsAdmin/NattLua))

- The profiling is continious, so you can refresh the static html page to get updates while it's running in the background
- You click on file paths to open them in your editor, assuming your editor supports uri schemes.
- Flamegraph view for the current time span view you're seeing
- You can select the time span you want to focus on
- Easily filter through traces and trace aborts

I'm looking for some feedback, especially when it comes to the UI design. The HTML portion is mostly vibe coded, though with extensive design feedback, so it's a bit hard to maintain by humans. I have mostly tested this on sessions that about ~20 seconds. I'm not sure how it scales to many minutes of data or even hours..

```lua
local Profiler = require('profiler')
-- config is optional, all values are default
local p = Profiler.New({
    path = "./output.html",
    file_url = "vscode://file/${path}:${line}:1",
    flush_interval = 3, -- in seconds
    sampling_rate = 1, -- the jit profiler sampling rate in ms
    depth = 500, -- the depth of the callstack when the jit profiler interupts. should be high for a detailed flamegraph.
}) 
do
    -- some expensive code here

    p:StartSection("foo")
    -- something else here
    p:StopSection()
end
p:Stop()
```

<img width="812" height="1070" alt="image" src="https://github.com/user-attachments/assets/d83823d0-e312-4f67-9801-7d7eea133755" />
