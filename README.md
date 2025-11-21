# 💉 Simple Windows DLL Injector

A straightforward **Windows DLL Injector** implemented in C++. This tool is designed to find a target process, allocate memory within it, write the path to a specified DLL into that memory, and execute it using `CreateRemoteThread` and `LoadLibraryA`.

## ⚙️ How It Works

This injector performs the following steps:

1.  **Find Process ID:** Uses the `GetProcID` function and the Windows API (`CreateToolhelp32Snapshot`, `Process32First/Next`) to find the Process ID (`prodID`) of the target executable (`iw4mp.exe`).
2.  **Get DLL Path:** The `GetDLLPath` function constructs the full path to the DLL, assuming the injector executable and the DLL (`dismay's mw2 internal.dll`) are in the **same directory**.
3.  **Open Process:** Opens a handle to the target process using `OpenProcess(PROCESS_ALL_ACCESS, ...)`
4.  **Allocate Memory:** Allocates memory inside the target process using `VirtualAllocEx` to hold the DLL path string.
5.  **Write Path:** Writes the DLL path string into the allocated memory using `WriteProcessMemory`.
6.  **Remote Thread:** Creates a remote thread in the target process, executing the `LoadLibraryA` function with the memory address of the DLL path as its argument. This forces the target process to load and execute the DLL.

## 📝 Configuration

* **Target Process:** Currently hardcoded as `const char* procName = "iw4mp.exe";` *can be changed*.
* **DLL Name:** Currently hardcoded as `path += "\\dismay's mw2 internal.dll";` *can be changed*.

**Note:** The injector continuously tries to find the target process in a loop until it succeeds.

This for written quickly for a [friend](https://github.com/0xKale)'s private tool for MW2
