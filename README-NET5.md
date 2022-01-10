[**SharpAvi.Net5**](https://github.com/EliahKagan/SharpAvi/tree/net5) is a
port, of [SharpAvi](https://github.com/baSSiLL/SharpAvi)
([nuget](https://www.nuget.org/packages/SharpAvi/)) by Vasili Maslov, to .NET 5
and higher (`net5-windows`). The minor changes done here to port it were done
by Eliah Kagan.

After the upstream SharpAvi project adds support for .NET 5/6, no further
versions of this unofficial port are planned, and users of this port should
strongly consider migrating to the upstream (official) version. But even
if/when that has happened, please feel free to contact me at any time about
questions/problems with this port.

[SharpAvi.Net5 is available as a NuGet package.](https://www.nuget.org/packages/SharpAvi.Net5)

## Other Ports

The following is a brief comparison of this unofficial port of SharpAvi to some
other unofficial ports.

If you&rsquo;re targeting .NET 5+ but you don&rsquo;t need Motion JPEG or MP3 audio, you
might be interested in
[SharpAvi.NetStandard](https://github.com/nguyenvuduc/SharpAvi)
([nuget](https://www.nuget.org/packages/SharpAvi.NetStandard/)), which is an
earlier port, by Duc Vu Nguyen, to .NET Standard 2.0. Compared to this port,
that one:

- can be consumed from a wider variety of targets (not just .NET 5 and higher).
- does not depend on Windows Presentation Foundation.
- pulls in fewer NuGet dependencies, because it does not use Roslyn.

The main situations when you might prefer to use this are if you want:

- [Features of SharpAvi
  2.2.1.](https://github.com/baSSiLL/SharpAvi/releases/tag/v2.2.1) As of this
  writing, SharpAvi.NetStandard only goes up to version 2.1.2.
- Motion JPEG video. SharpAvi depends on WPF to encode Motion JPEG, which
  appears to be why SharpAvi.NetStandard omits this functionality.
- MP3 audio. SharpAvi uses CodeDom to compile a managed-code facade at runtime
  that interoperates with native [LAME](https://lame.sourceforge.io/) binaries
  (and SharpAvi.NetStandard doesn&rsquo;t modify this code). [CodeDom
  doesn&rsquo;t support compilation on .NET Core and .NET
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

Finally, you may wish to consider using [Jérémy Ansel&rsquo;s modified
version](https://github.com/JeremyAnsel/SharpAvi/tree/net_standard) (proposed
in [this pull request](https://github.com/baSSiLL/SharpAvi/pull/27)) instead of
this one. That port to .NET Standard 2.0 retains Motion JPEG support by
removing its dependence on WPF.

## Acknowledgements

I&rsquo;d like to thank:

- **Vasili Maslov**, for writing SharpAvi!
- **Michael Kagan**, for helping me to test this port with various codecs,
  configurations, and use cases.
