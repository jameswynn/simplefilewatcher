SimpleFileWatcher is a C++ wrapper for OS file monitoring systems. Currently
it uses Win32 ReadDirectoryChangesW for monitoring changes in Windows,
and inotify in linux. OSX is supported via kqueue and directory scans.

Some example code:
```
    // Create the object
    FW::FileWatcher* fileWatcher = new FW::FileWatcher();

    // add a directory watch
    FW::WatchID watchid = fileWatcher->addWatch("..\\media", new UpdateListener());

    ...

    // somewhere in your update loop call update
    fileWatcher->update();

    // where UpdateListener is defined as such
    class UpdateListener : public FW::FileWatchListener
    {
    public:
       UpdateListener() {}
       void handleFileAction(FW::WatchID watchid, const String& dir, const String& filename,
                   FW::Action action)
       {
          switch(action)
          {
          case FW::Actions::Add:
             std::cout << "File (" << dir + "\\" + filename << ") Added! " <<  std::endl;
             break;
          case FW::Actions::Delete:
             std::cout << "File (" << dir + "\\" + filename << ") Deleted! " << std::endl;
             break;
          case FW::Actions::Modified:
             std::cout << "File (" << dir + "\\" + filename << ") Modified! " << std::endl;
             break;
          default:
             std::cout << "Should never happen!" << std::endl;
       }
    };
```


A sample integrating SFW with [Ogre3D](http://www.ogre3d.org) is packaged with the source.