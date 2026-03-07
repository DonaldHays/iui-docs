# Installation
IUI itself is engine-agnostic. It doesn't know how to receive input or draw to the screen. Backend libraries connect IUI to game engines. Thus, you need to install both the IUI library and a backend library to use the toolkit.

**Core Library**
- [iui](<https://github.com/DonaldHays/iui>): required regardless which backend you choose

**Backends**
- [love-iui](<https://github.com/DonaldHays/love-iui>): backend for LÖVE
- [lovr-iui](<https://github.com/DonaldHays/lovr-iui>): backend for LÖVR

**Sample Projects**
- [iui-sample-love](<https://github.com/DonaldHays/iui-sample-love>): sample project for LÖVE
- [iui-sample-lovr](<https://github.com/DonaldHays/iui-sample-lovr>): sample project for LÖVR, featuring both desktop and VR modes