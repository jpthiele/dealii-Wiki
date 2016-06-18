# Using QtCreator as your IDE

## Installation

Download the binary for your system from https://www.qt.io/download-open-source/#section-6

Download the deal.II indentation style from [qtcreator-dealii-format](https://docs.google.com/uc?id=0B1_zqLUYjm2eZV9YVkdsa3EtSm8&export=download) and set it under Tools->Options->C++->Code style->Import.

If you use a cmake build that is installed in your local user acccount, it needs to be set under settings->build&run->cmake.

In your default kit (settings->build&run->kits) select Qt version "none". Otherwise some reserved words like "signal" are not parsed correctly by qtcreator.

You can download [dealii-prm.xml](https://drive.google.com/open?id=0B1_zqLUYjm2edkVIbW5KVUpqdDg) and put it into 
~/.config/QtProject/qtcreator/generic-highlighter to get syntax highlighting for .prm files.

## Usage

Open the CMakeLists.txt and then choose the existing build directory.

Note that version 3.6 and newer revamped the cmake support which means that setting up the project takes a few more clicks (uncheck all but one build types/directories).