# How to use most common licenses

In general licenses share at least one point, and that is "Keep the copyright notice". The least thing you can do - include copyright notice to documentation or EULA. For example: "This software uses `library name` - see library-license.txt".
  
## MIT license

* Keep the copyright notice
* You are free to modify, redistribute and even sublicense the code under another license
* The original author isn’t liable for any damage resulting from his code

## BSD License

* Keep the copyright notice
* You are free to use, redistribute and license the code under another license

## Apache 2.0 License

* Keep the copyright notice
* Your software has to contain a copy of the Apache 2 license
* You are free to use, modify, distribute and redistribute the software
* If you modify code you have to mention your modifications particularly
* If there is a text file called NOTICE: Read it! It contains further information about specific parts of the license and the software’s purpose
* The NOTICE file has to be included in your software release too

## GPL License

* Keep the licensing header
* Your software release has to be GPL licensed too
* If anyone requests it, you have to make the sources available

## MPL 2.0

* Keep the copyright notice
* Provide link to the source for the MPLed code
* If you modify code you must make available the MPL-licensed portions of the source cod, and inform the recipients how they can obtain it.

# Theory

## Public Domain

Public domain equivalent license are licenses that grant public-domain-like rights or/and act as waivers. They are used to make copyrighted works usable by anyone without conditions, while avoiding the complexities of attribution or license compatibility that occur with other licenses. However in different countries **Public Domain** concept can be undefined or defined in different ways, so usage of this license can bring some risks.

### Zero / public domain (CC0)

[CC0](https://en.wikipedia.org/wiki/Creative_Commons_license#Zero_.2F_public_domain)  was created for compatibility with law domains which have no concept of dedicating into public domain. This is achieved by a public domain waiver statement and a fall-back to all-permissive license.

## Permissive software license

A permissive software license, sometimes also called BSD-like or BSD-style license, is a free software licence with minimal requirements about how the software can be redistributed. Such licenses requiring little more than attributing the original portions of the licensed code to the original developers in your own code and/or documentation.

Examples of permissive licenses: [MIT License](https://en.wikipedia.org/wiki/MIT_License), [BSD licences](https://en.wikipedia.org/wiki/BSD_licenses), [Apple Public Source License](https://en.wikipedia.org/wiki/Apple_Public_Source_License) and the [Apache licence](https://en.wikipedia.org/wiki/Apache_License).

## Copyleft software license

Copyleft is a general method for making a program (or other work) free (in the sense of freedom, not “zero price”), and requiring all modified and extended versions of the program to be free as well. 

Copyleft licenses for software require that information necessary for reproducing and modifying the work must be made available to recipients of the binaries

What does it mean in practice? 

If you do distribute your application, and you used some Copyleft library as part of your application (even if only linking at run-time to a library) - and even if you do not charge money - and even if you do not change that library in any way - then you MUST make the source of your application available for end users.

Making source available does not mean that users can download it automatically. It might be that you must get a written request and then you send sources. You can not escape the obligation to make your own source code available to end users.

Examples of copyleft licenses: [GPL Licenses](https://en.wikipedia.org/wiki/GNU_General_Public_License), [IBM Public License](https://en.wikipedia.org/wiki/IBM_Public_License)

## Hybrids (combination of Permissive and Copyleft licenses)

[MPL v2.0 (Mozilla Public License Version 2.0)](https://www.mozilla.org/en-US/MPL/2.0/) is a copyleft license that is easy to comply with. You can combine the MPL v2.0 software with proprietary code.

* If you don't modify it - you have to provide just link to the library sources. It looks like Permissive license.
* If you modify it - you have to provide sources for modified files (not whole project). It looks like weak Copyleft license.

## License compatibility

![License compatibility](https://upload.wikimedia.org/wikipedia/commons/1/1d/Floss-license-slide-image.png)

License compatibility between common software licences: the vector arrows denote a one directional compatibility, therefore better compatibility on the left side ("permissive licenses") than on the right side ("copyleft licences")

## Comparison of free and open-source software licenses

Refer to [Comparison of free and open-source software licenses](https://en.wikipedia.org/wiki/Comparison_of_free_and_open-source_software_licenses) to find out if the license compatible with your project.

