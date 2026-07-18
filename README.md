# AvaloniaSimpleSingleFileAOT
A simple **single-file** Avalonia NativeAOT application that uses static library linking, with an executable size of ~32 MB.

### Build environment:
- .NET 10.0
- Avalonia 12.1.0

### Static libraries:
- SkiaSharp & HarfBuzzSharp: https://github.com/2ndlab/SkiaSharp.Static
- av_libglesv2: https://github.com/2ndlab/ANGLE.Static

### Usage:
[AvaloniaApplication1.csproj](https://github.com/Hooooot/AvaloniaSimpleSingleFileAOT/blob/master/AvaloniaApplication1/AvaloniaApplication1.csproj)

#### Key Points
Add the following options to the <PropertyGroup> section:
```
<PublishAot>true</PublishAot>
<RuntimeIdentifier>win-x64</RuntimeIdentifier>
```
and add the following code to <Project> section:
```xml
    <ItemGroup Label="ImportLib"
			   Condition="'$(PublishAot)' == 'true' and '$(RuntimeIdentifier)' == 'win-x64'">
		
		<LinkerArg Include="/LTCG" />
		
		<!-- HarfBuzzSharp -->
		<DirectPInvoke Include="libHarfBuzzSharp" />
		<NativeLibrary Include="$(MSBuildProjectDirectory)\Native\libHarfBuzzSharp.lib" />

		<!-- SkiaSharp -->
		<DirectPInvoke Include="libSkiaSharp" />
		<NativeLibrary Include="$(MSBuildProjectDirectory)\Native\SkiaSharp.lib" />
		<NativeLibrary Include="$(MSBuildProjectDirectory)\Native\skia.lib" />

		<!-- Avalonia ANGLE/GLES -->
		<DirectPInvoke Include="av_libglesv2" />
		<NativeLibrary Include="$(MSBuildProjectDirectory)\Native\libANGLE_static.lib" />
		<NativeLibrary Include="$(MSBuildProjectDirectory)\Native\libGLESv2_static.lib" />

		<!-- Windows SDK import libraries -->
		<NativeLibrary Include="Synchronization.lib" />
		<NativeLibrary Include="Gdi32.lib" />
		<NativeLibrary Include="DXGI.lib" />
		<NativeLibrary Include="d3d9.lib" />
		<NativeLibrary Include="dxguid.lib" />
	</ItemGroup>
```
