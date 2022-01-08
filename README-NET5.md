*Note: After the upstream SharpAvi project adds support for .NET 5/6, no
further versions of this unofficial port are planned, and users of this port
should strongly consider migrating to the upstream (official) version.*

**SharpAvi.Net5** is a port, of [SharpAvi](https://github.com/baSSiLL/SharpAvi)
([nuget](https://www.nuget.org/packages/SharpAvi/)) by Vasili Maslov, to .NET 5
and higher (`net5-windows`). The minor changes done here to port it were done
by Eliah Kagan.

[SharpAvi.Net5 is available as a NuGet package.](https://www.nuget.org/packages/SharpAvi.Net5)

If you're targeting .NET 5+ but you don't need Motion JPEG or MP3 audio, you
may prefer [SharpAvi.NetStandard](https://github.com/nguyenvuduc/SharpAvi)
([nuget](https://www.nuget.org/packages/SharpAvi.NetStandard/)), which is an
earlier port, by Duc Vu Nguyen, to .NET Standard 2.0. Compared to this port,
that one:

- can be consumed from a wider variety of targets (not just .NET 5 and higher).
- does not depend on Windows Presentation Foundation.
- pulls in fewer NuGet dependencies, because it does not use Roslyn.

The main situations when you might prefer to use this are if you want:

- Motion JPEG video. SharpAvi depends on WPF to encode Motion JPEG, which
  appears to be why SharpAvi.NetStandard omits this functionality.
- MP3 audio. SharpAvi uses CodeDom to compile a managed-code facade at runtime
  that interoperates with native [LAME](https://lame.sourceforge.io/) binaries
  (and SharpAvi.NetStandard doesn't modify this code). [CodeDom doesn't support
  compilation on .NET Core and .NET
  5+.](https://docs.microsoft.com/en-us/dotnet/core/compatibility/unsupported-apis#systemcodedomcompiler)
- The ported (.NET 5) sample application.

Porting was accomplished by:

1. Running the [.NET Upgrade
   Assistant](https://aka.ms/dotnet-upgrade-assistant-overview).
2. Rewriting a single short
   method&mdash;`SharpAvi.Codecs.GenerateLameFacadeAssembly` in
   [`SharpAvi/Codecs/Mp3AudioEncoderLame.cs`](https://github.com/EliahKagan/SharpAvi/blob/2.1.2-net5/SharpAvi/Codecs/Mp3AudioEncoderLame.cs#L65)&mdash;to
   use Roslyn instead CodeDom for runtime code generation.

Unfortunately, this code generation task with Roslyn is markedly slower than it
was with CodeDom.

This port is released under the same license as SharpAvi itself (the MIT
license).

Finally, you may wish to consider using [Jérémy Ansel's modified
version](https://github.com/JeremyAnsel/SharpAvi/tree/net_standard) (proposed
in [this pull request](https://github.com/baSSiLL/SharpAvi/pull/27)) instead of
this one. That port to .NET Standard 2.0 retains Motion JPEG support by
removing its dependence on WPF.
