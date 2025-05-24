---
title: VisualAssist设置
description:
categories:
 - VisualAssist
tags:
---

# 1. 设置VisualAssist颜色Teal, Gray, C000C0, C00000
# 2. 修改VisualAssist宏 #ifdef guard in a header的快捷键为#head
内容如下:
```
#ifndef __$FILE_BASE_UPPER$_H_INCLUDE__
#define __$FILE_BASE_UPPER$_H_INCLUDE__

$selected$

#endif // __$FILE_BASE_UPPER$_H_INCLUDE__
```
# 3. Highlight references Blue 400000

# 4. 快捷键 #env

```
// --- VAX Snippet Variables Test (Simplified Version) ---
// Please make sure to trigger this Snippet in a C++ file that is [saved]
// and part of an [open project and solution].
// The correct expansion of many variables highly depends on these conditions. Variable names are case-sensitive.

// ==============================================
// File and Path Related Variables
// ==============================================
// Current file name (without extension): $FILE_BASE$
// Current file name (with extension): $FILE_BASE$.$FILE_EXT$
// Current file extension (e.g., .cpp, usually includes dot): $FILE_EXT$
// Current file full path: $FILE_PATH$\$FILE_BASE$.$FILE_EXT$
// (You previously reported that $FILE_BASE_UPPER$ or similar uppercase filename variables worked for you; you can add tests for it here yourself)
// Current project name: $PROJECT_NAME$
// Current solution name: $SOLUTION_NAME$

// ==============================================
// Date and Time Related Variables
// (Dynamically generated based on the time of your test)
// ==============================================
// Current day (DD, e.g., 05, 23): $DAY$
// Current month (MM, e.g., 01, 12): $MONTH$
// Current year (YYYY, e.g., 2025): $YEAR$
// Current year (YY, e.g., 25): $YEAR_02$
// System date format (e.g., 2025/05/23): $DATE$

// Current hour (HH, 24-hour format, e.g., 08, 17): $HOUR$
// Current minute (MI, e.g., 05, 59): $MINUTE$
// Current second (SS, e.g., 00, 35): $SECOND$

// ==============================================
// User and System Related Variables
// ==============================================
// New GUID (Globally Unique Identifier): $GUID_SYMBOL$ {$GUID_STRING$} 
// Current Windows Username: %USERNAME%
// Current Windows User Domain: %USERDOMAIN%
// Computer Name: %COMPUTERNAME%
// (Note: %VAR% style is for Windows environment variables. If these work in your VAX snippets.)

// ==============================================
// Editor and Code Context Related Variables
// ==============================================

// --- The following variables are more dependent on the specific code context where the Snippet is inserted ---
// Current class name (requires being within a class scope): $ClassName$
// Current method name (requires being within a method scope): $MethodName$

// --- Snippet End and Cursor Position ---
// VAX will place the cursor here (this line itself will be replaced by cursor placement):
$end$
```

# 5. 快捷键 #interface

```

// Copyright (c) 2018 brinkqiang (brink.qiang@gmail.com)
//
// Permission is hereby granted, free of charge, to any person obtaining a copy
// of this software and associated documentation files (the "Software"), to deal
// in the Software without restriction, including without limitation the rights
// to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
// copies of the Software, and to permit persons to whom the Software is
// furnished to do so, subject to the following conditions:
//
// The above copyright notice and this permission notice shall be included in all
// copies or substantial portions of the Software.
//
// THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
// IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
// FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
// AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
// LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
// OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
// SOFTWARE.


#ifndef __$FILE_BASE_UPPER$_H_INCLUDE__
#define __$FILE_BASE_UPPER$_H_INCLUDE__

#include "dmos.h" 
#include <string>
#include <vector>
#include <cstddef> 

class I$FILE_BASE_LOWER$ {
public:
    virtual ~I$FILE_BASE_LOWER$() {}
    virtual void DMAPI Release(void) = 0;
};

extern "C" DMEXPORT_DLL I$FILE_BASE_LOWER$* DMAPI $FILE_BASE_LOWER$GetModule();
typedef I$FILE_BASE_LOWER$* (DMAPI* PFN_$FILE_BASE_LOWER$GetModule)();

#endif

```