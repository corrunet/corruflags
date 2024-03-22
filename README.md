# Why not just use [Aikar](https://github.com/aikar)'s flags?
His GC is based on G1, it's extremely stable but extremely slow, so why should current gen servers be held back by last gen algorithms?

I propose to get rid of it in favor of [ZGC](https://github.com/openjdk/zgc), why? It clears memory alongside the server instead of pausing it each time, and it's pretty fast.

# Who should use these flags?
Generally modded servers or large networks will probably benefit the most from performance flags.

Smaller servers or clients with limited resources could also see major uplifts with stability flags.

When using these flags you should try to use [GraalVM](https://www.graalvm.org/) as your java runtime ***if possible***

Some versions have [ZGC](https://github.com/openjdk/zgc) support and some don't, do research, do testing. If you can't get it working with performance flags then just run it with your pre existing java runtime or see if stability flags provide more performance with [GraalVM](https://www.graalvm.org/) than your current runtime with performance flags.

# Heads Up
Do not rely on these flags as your only source of optimization, yes they could theoretically increase your performance up to 50%, but to be brutally honest it's snake oil compared to optimizing your server properly, use this as a cherry on top in terms of performance, not the entire cake itself.

I will have an advanced guide out soon for players who want the absolute best performance with no regard to stability, which is probably not a really good idea but I mean if your absolutely crazy like I am, just go for gold you got nothing to lose. in the meantime use [this](https://github.com/YouHaveTrouble/minecraft-optimization) guide.

# Flags
- [x] [Vanilla](https://www.minecraft.net/en-us/download/server) (Not recommended, use [Leaf](https://github.com/Winds-Studio/Leaf), [Mirai](https://github.com/Dreeam-qwq/Mirai) or [Paper](https://github.com/PaperMC/Paper).)
- [x] [Leaf](https://github.com/Winds-Studio/Leaf) / [Mirai](https://github.com/Dreeam-qwq/Mirai) / [Paper](https://github.com/PaperMC/Paper)
- [x] [Folia](https://github.com/PaperMC/Folia)
- [x] [Fabric](https://github.com/FabricMC)
- [x] [Quilt](https://github.com/QuiltMC)
- [x] [Forge](https://github.com/MinecraftForge/MinecraftForge)
- [x] [NeoForge](https://github.com/neoforged/NeoForge)
- [x] Most JVM software, even non minecraft (No guarentee, not recommended.)

***Performance Focused / Server / More than 12GB, ZGC:***

Use these flags only on a server that needs performance that has more than 12GB available. 

(Not suggested for clients unless you have an extremely high class machine)

```java
java -Xms12G -Xmx12G -XX:+UnlockExperimentalVMOptions -XX:+UnlockDiagnosticVMOptions -XX:+UseZGC -XX:-ZUncommit -XX:-ZProactive -Dterminal.jline=false -Dterminal.ansi=true -Djline.terminal=jline.UnsupportedTerminal -Dlog4j2.formatMsgNoLookups=true -XX:+AlwaysActAsServerClassMachine -XX:+AlwaysPreTouch -XX:+DisableExplicitGC -XX:+UseNUMA -XX:NmethodSweepActivity=1 -XX:ReservedCodeCacheSize=400M -XX:NonNMethodCodeHeapSize=12M -XX:ProfiledCodeHeapSize=194M -XX:NonProfiledCodeHeapSize=194M -XX:+PerfDisableSharedMem -XX:+UseFastUnorderedTimeStamps -XX:+UseCriticalJavaThreadPriority -XX:+EagerJVMCI -Dgraal.TuneInlinerExploration=1 -Dgraal.CompilerConfiguration=enterprise -XX:+ParallelRefProcEnabled -XX:InitiatingHeapOccupancyPercent=15 -XX:SurvivorRatio=32 -XX:MaxTenuringThreshold=1 -XX:-UseBiasedLocking -XX:+UseStringDeduplication -XX:+UseAES -XX:+UseAESIntrinsics -XX:+UseFMA -XX:+UseLoopPredicate -XX:+RangeCheckElimination -XX:+EliminateLocks -XX:+DoEscapeAnalysis -XX:+UseCodeCacheFlushing -XX:+SegmentedCodeCache -XX:+UseFastJNIAccessors -XX:+OptimizeStringConcat -XX:+UseCompressedOops -XX:+UseThreadPriorities -XX:+OmitStackTraceInFastThrow -XX:+TrustFinalNonStaticFields -XX:ThreadPriorityPolicy=1 -XX:+UseInlineCaches -XX:+RewriteBytecodes -XX:+RewriteFrequentPairs -XX:-DontCompileHugeMethods -XX:+UseFPUForSpilling -XX:AllocatePrefetchStyle=1 -XX:+UseVectorCmov -XX:+UseXMMForArrayCopy -XX:+UseLargePages -XX:LargePageSizeInBytes=2M -Dfile.encoding=UTF-8 -Xlog:async -Djava.security.egd=file:/dev/urandom --add-modules jdk.incubator.vector -jar server.jar –-nogui
```

***Stability Focused / Client, Server / Less than 12GB, G1GC:***

Use these flags on a server that prefers stability over performance, or has less than 12GB available 

(Clients *can* work however not tested properly)

```java
java -Xms8G -Xmx8G -XX:+UnlockExperimentalVMOptions -XX:+UnlockDiagnosticVMOptions -XX:+UseG1GC -XX:G1NewSizePercent=30 -XX:G1MaxNewSizePercent=40 -XX:G1HeapRegionSize=8M -XX:G1ReservePercent=20 -XX:G1HeapWastePercent=5 -XX:G1MixedGCCountTarget=4 -XX:G1MixedGCLiveThresholdPercent=90 -XX:G1RSetUpdatingPauseTimePercent=5 -Dterminal.jline=false -Dterminal.ansi=true -Djline.terminal=jline.UnsupportedTerminal -Dlog4j2.formatMsgNoLookups=true -XX:+AlwaysActAsServerClassMachine -XX:+AlwaysPreTouch -XX:+DisableExplicitGC -XX:+UseNUMA -XX:AllocatePrefetchStyle=3 -XX:NmethodSweepActivity=1 -XX:ReservedCodeCacheSize=400M -XX:NonNMethodCodeHeapSize=12M -XX:ProfiledCodeHeapSize=194M -XX:NonProfiledCodeHeapSize=194M -XX:+PerfDisableSharedMem -XX:+UseFastUnorderedTimeStamps -XX:+UseCriticalJavaThreadPriority -XX:+EagerJVMCI -Dgraal.TuneInlinerExploration=1 -Dgraal.CompilerConfiguration=enterprise -XX:+ParallelRefProcEnabled -XX:MaxGCPauseMillis=200 -XX:InitiatingHeapOccupancyPercent=15 -XX:SurvivorRatio=32 -XX:MaxTenuringThreshold=1 -XX:-UseBiasedLocking -XX:+UseStringDeduplication -XX:+UseAES -XX:+UseAESIntrinsics -XX:+UseFMA -XX:+UseLoopPredicate -XX:+RangeCheckElimination -XX:+EliminateLocks -XX:+DoEscapeAnalysis -XX:+UseCodeCacheFlushing -XX:+SegmentedCodeCache -XX:+UseFastJNIAccessors -XX:+OptimizeStringConcat -XX:+UseCompressedOops -XX:+UseThreadPriorities -XX:+OmitStackTraceInFastThrow -XX:+TrustFinalNonStaticFields -XX:ThreadPriorityPolicy=1 -XX:+UseInlineCaches -XX:+RewriteBytecodes -XX:+RewriteFrequentPairs -XX:-DontCompileHugeMethods -XX:+UseFPUForSpilling -XX:+UseVectorCmov -XX:+UseXMMForArrayCopy -XX:+UseLargePages -XX:LargePageSizeInBytes=2M -Dfile.encoding=UTF-8 -Xlog:async -Djava.security.egd=file:/dev/urandom --add-modules jdk.incubator.vector -jar server.jar –-nogui
```
> Warning `Option UseBiasedLocking was deprecated in version 15.0 and will likely be removed in a future release.` can be completely ignored without consequences, it won't effect anything and is unlikely to be removed anytime soon.

*Don't forget to adjust -Xms and -Xmx to your server configuration, along with java path and server.jar if applicable*

**Do not allocate more than 32GB on stability flags, G1GC will start to struggle. Performance flags may go as high as they need but not lower than 12GB.** 

*(according to documentation, untested)*

# Explanation

***Please try not to rely on these explanations, do your own research as I don't know how to explain things due to my anxiety***

> -Xms8G -Xmx8G

Sets the minimum and maximum memory usage, obvious. Always set these values to the same number as unused ram is wasted ram in most cases and it increases performance consistancy.

> -XX:+UnlockExperimentalVMOptions -XX:+UnlockDiagnosticVMOptions

Allows for the use of most of the flags here.

> -XX:+UseZGC -XX:-ZUncommit

Tells java while using performance flags to use ZGC and forces it to never give allocated memory back to the OS to improve speeds.

> -XX:+UseG1GC -XX:G1NewSizePercent=30 -XX:G1MaxNewSizePercent=40 -XX:G1HeapRegionSize=8M -XX:G1ReservePercent=20 -XX:G1HeapWastePercent=5 -XX:G1MixedGCCountTarget=4 -XX:G1MixedGCLiveThresholdPercent=90 -XX:G1RSetUpdatingPauseTimePercent=5 -XX:MaxGCPauseMillis=200

Tells java while using stability flags to use G1GC and make short pauses instead of long ones.

> -XX:+AlwaysActAsServerClassMachine

In certain hardware configurations (lower than 2g) java may force SerialGC which is much slower than G1GC or ZGC, this forces java to always think of the machine as "Server Class"

> -XX:+AlwaysPreTouch

Allocates memory directly to the JVM instead of virtual memory.

> -XX:+DisableExplicitGC

Prevents mods/plugins from performing garbage collection on it's own.

> -XX:+UseNUMA

Tells JVM that the system uses Non-uniform Memory Access (NUMA), increasing the use of lower latency memory on multi-node topologies (Caches or RAM). Only available when used with `-XX:+UseParallelGC`

> -XX:AllocatePrefetchStyle=3

Allows the JVM to prefetch instructions.

> -XX:+PerfDisableSharedMem

Disables other processes from using memory that belongs to the JVM.

> XX:+ParallelRefProcEnabled

Allows for parallel reference processing whenever possible.

> -XX:-UseBiasedLocking

Improves performance of un-contended synchronization, also spits an error on startup about depreciation which you ***should ignore***

> -XX:+UseStringDeduplication

Saves some memory by referencing similar string objects instead of keeping duplicates.

> XX:+UseAES -XX:+UseAESIntrinsics -XX:+UseFMA

Enables AES/FMA which very slightly improves performance however is incompatible on >2009 CPUS. Unintentionally also fixes a [Geyser bug](https://github.com/GeyserMC/Geyser/issues/3490)

> -XX:+UseCodeCacheFlushing

Removes old methods from cache

> -XX:+UseThreadPriorities

Allows a JVM app like minecraft or mods/plugins to set priority of a thread, good for Folia, C2ME or DimThread

> -jar server.jar

Tells java what the name of the jar executable is

> –-nogui

Forces games like minecraft to not display the funky gui, does not matter in non graphical enviroments, I personally remove this flag when running JVM servers on my machine.

> -Dterminal.jline=false -Dterminal.ansi=true -Djline.terminal=jline.UnsupportedTerminal Dlog4j2.formatMsgNoLookups=true -XX:NmethodSweepActivity=1 -XX:ReservedCodeCacheSize=400M -XX:NonNMethodCodeHeapSize=12M -XX:ProfiledCodeHeapSize=194M -XX:NonProfiledCodeHeapSize=194M -XX:+UseFastUnorderedTimeStamps -XX:+UseCriticalJavaThreadPriority -XX:+EagerJVMCI -Dgraal.TuneInlinerExploration=1 -Dgraal.CompilerConfiguration=enterprise -XX:InitiatingHeapOccupancyPercent=15 -XX:SurvivorRatio=32 -XX:MaxTenuringThreshold=1 -XX:+UseLoopPredicate -XX:+RangeCheckElimination -XX:+EliminateLocks -XX:+DoEscapeAnalysis -XX:+SegmentedCodeCache -XX:+UseFastJNIAccessors -XX:+OptimizeStringConcat -XX:+UseCompressedOops -XX:+OmitStackTraceInFastThrow XX:+TrustFinalNonStaticFields -XX:+UseInlineCaches -XX:+RewriteBytecodes -XX:+RewriteFrequentPairs -XX:-DontCompileHugeMethods -XX:+UseFPUForSpilling -XX:+UseVectorCmov -XX:+UseXMMForArrayCopy Dfile.encoding=UTF-8 -Xlog:async -Djava.security.egd=file:/dev/urandom --add-modules jdk.incubator.vector -XX:+UseLargePages -XX:LargePageSizeInBytes=2M -XX:ThreadPriorityPolicy=1

These flags are generally either [GraalVM](https://www.graalvm.org/) specific optimizations which optimize performance, or they make such little difference in how a JVM app works that they don't deserve documentation on this page for the sake of my own sanity, do your own research.

# Notice
These flags aren't as rigorously tested as something like Aikar's Flags, although the chances are extremely slim, you could encounter problems like crashing or in the absolute worst case scenario, even corruption.

***USE THEM AT YOUR OWN RISK!*** v2k or anybody affiliated is not responsible for any damages as a result of these flags being used, You are on your own and no support will be given.
