# Getting Started #

To add SFW to an existing project, simply add one or all of the implementation files from the source directory to the project. Unused implementations will get ignored by the precompiler, so they won't interfere.

Then add the headers to the includes path. I prefer putting the whole subdirectory into the path to allow the synatx:
```
#include <FileWatcher/FileWatcher.h>
```

Then you need a class that inherits from `FW::FileWatchListener` and implements `handleFileAction`:
```
class UpdateListener : public FW::FileWatchListener
{
public:
	UpdateListener() {}
	void handleFileAction(FW::WatchID watchid, const FW::String& dir, const FW::String& filename,
		FW::Action action)
	{
		std::cout << "DIR (" << dir + ") FILE (" + filename + ") has event " << action << std::endl;
	}
};
```

Your program will need an instance of ```FW::FileWatcher``` and to register a listener for each of the directories you want to watch:
```
FW::FileWatcher fileWatcher;
fileWatcher.addWatch("./test", new UpdateListener());
```

Then periodically, call `FW::FileWatcher.update()`.

Also, you will likely want to wrap both `addWatch` and `update` in a try...catch statement, as these will throw exceptions on error.