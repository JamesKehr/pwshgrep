# pwshgrep
pwshgrep stands for PowerShell grep. This PowerShell module adds grep-like features to PowerShell.

# What is grep?
grep is a popular Linux/Unix/macOS (Linux) command line tool used to quickly search through text. Some might even call it a critical part of the Linux command line experience.

The Windows equal is findstr or Select-String, but both of these tools don't quite have the grep speed or feel. So, I made my own.

# Do I have to type pwshgrep to use it?

No. The module loads a function named grep that can be used in the PowerShell pipeline. This module is currently only built to be used with Windows, which lacks grep and therefore does not conflict with another name like it would in Linux.

# How does pwshgrep differ from grep?
pwshgrep Is built to be more object-oriented than grep, which is purely text based. The output of pwshgrep is a collection (array) or objects. The object depends on the type of search that was performed.

pwshgrep also exclusively uses regex matches, where regex is optional with grep. Simple searches, like for a word, work fine with regex matching. It's simply fast and efficient, making pwshgrep perform more grep-like.

# How does it work?
pwshgrep has two modes: string and path

## String mode
This is the most grep-like mode. A string is searched and the matches are returned. All searches are case-insensitive.

```powershell
# Example 1: A simple text search
 @'
This is a test string.
This is only a test.
Had this string been production it would be stored in a file.
But it's not.
So it isn't.
And that makes it a test.
'@ | grep "test"
```
Output:
```
Line Result
---- ------
   0 This is a test string.
   1 This is only a test.
   5 And that makes it a test.
```
```powershell
# Example 2: Search inside of a file using Get-Content (alias: cat)
cat C:\temp\PktMon.txt | grep "ICMP"
```
Output:
```
 Line Result
 ---- ------
 1245 [00]0004.0300::2025-01-08 15:00:45.839956300 [Microsoft-Windows-PktMon] Drop: PktGroupId 0, PktNumber 1, Appeara…
 1352 [00]0004.0300::2025-01-08 15:00:45.874040400 [Microsoft-Windows-PktMon] Drop: PktGroupId 0, PktNumber 1, Appeara…
 4384 [00]0004.0300::2025-01-08 15:00:48.839690200 [Microsoft-Windows-PktMon] Drop: PktGroupId 0, PktNumber 1, Appeara…
 4467 [00]0004.0300::2025-01-08 15:00:48.874406100 [Microsoft-Windows-PktMon] Drop: PktGroupId 0, PktNumber 1, Appeara…
```
```powershell
"test", "nope", "huh", "another test" | grep test
```
Output:
```
Line Result
---- ------
   0 test
   3 another test
```

## File mode
This mode will scan one or more text-based files for matches to a pattern. This mode accepts either a single file or a directory. Only the parent directory is searched unless -Recurse (alias: -r) is used.

The default file extension is TXT. Use -Include to search through multiple file extensions. Or, use file scenarios like -PowerShell (aliases: -pwsh -ps) (see wiki (coming soon(TM)).

```powershell
gi scripts:\ | grep "pwshgrep" -Recurse -PowerShell
```
Output:
```
ShortPath                Line Result
---------                ---- ------
..pwshgrep\pwshgrep.psm1    7 pwshgrep is a module that will [eventually] contain two tools: grep and ogrep.
..pwshgrep\pwshgrep.psm1   17     pwshgrep cannot act perfectly like grep because PowerShell is object based and not t…
..pwshgrep\pwshgrep.psm1   47    - Using any param from the Path set name automatically switches pwshgrep to Path.
..pwshgrep\pwshgrep.psm1   66 - DONE - Add OpenFile() to [pwshgrepFileResult].
..pwshgrep\pwshgrep.psm1   73   - Add stats to pwshgrepFileResult
..pwshgrep\pwshgrep.psm1   74   - Move the file search code to pwshgrepFileResult
..pwshgrep\pwshgrep.psm1   96 class pwshgrepFileResult {
```

# Why use this over File/Windows Explorer?

It is exponentially faster for plain text file searches. It also returns the file path, the exact line number (zero-indexed) in the file where the match occured, and the matching line.

The one real advantage that explorer has is that it can search inside of archive files (ZIP) and Office documents. pwshgrep is limited to plain text files.
