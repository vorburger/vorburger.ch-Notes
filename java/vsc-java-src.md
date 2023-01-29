# Java JDK Source Code in Visual Studio Code

I've switched from using Eclipse to Visual Studio Code (VSC) for Java coding,
and was missing the JDK Source Code in VSC.

[The discussion in this bug](https://github.com/redhat-developer/vscode-java/issues/689) didn't help, because the problem was the following:

It appears that the Language Support for Java by Red Hat CSV extension "helps" users with its (Ctrl-Shift-P) _Install New JDK_ UI feature. Those get installed into `~/.vscode/extensions/redhat.java-1.14.0-linux-x64/jre`, are entirely separate from any "system package" JDK installs - and do not appear to include the `src.zip` with JDK source code (at least some of them, such as _Eclipse Adoptium's Temurin)_.

The simplest solution, e.g. on _Fedora Workstation,_ is to instead of installing additional JDKs into VSC, just e.g. DNF install a Fedora JDK package e.g. with `sudo dnf install java-17-openjdk-devel java-17-openjdk-src`, and then add e.g. this to your `~/.config/Code/User/settings.json`, as per [vscode-java](https://github.com/redhat-developer/vscode-java#setting-the-jdk) and [VSC](https://code.visualstudio.com/docs/java/java-project#_configure-runtime-for-projects) documentation:

    "java.configuration.runtimes": [
      {
        "name": "JavaSE-17",
        "path": "/usr/lib/jvm/java-17/",
        "sources": "/usr/lib/jvm/java-17/lib/src.zip",
        "javadoc": "https://docs.oracle.com/en/java/javase/17/docs/api"
      }
    ]

PS: It would be nice not to have to point to `docs.oracle.com` for JavaDoc, but the `java-17-openjdk-javadoc` RPM package unfortunately seems to install only into an "unstable" directory name (like `/usr/share/javadoc/java-17-openjdk-17.0.5.0.8-1.fc37.x86_64/api`) with changing minor version numbers, and doesn't symlink into a fixed `/java-17/` path. (There is also a
`java-17-openjdk-javadoc-zip` package, but it seems to have the same problem.)
