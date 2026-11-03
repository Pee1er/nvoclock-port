Title: NVIDIA nvoclock.sys I/O port access

Vendor: NVIDIA Corporation
Product: NVidia System Utility Driver
Component: nvoclock.sys
SHA-256: 2203bd4731a8fdc2a1c60e975fd79fd5985369e98a117df7ee43c528d3c85958
MD5: d1e57c74bafa56e8e2641290d153f4d2
SHA-1: 007b2c7d72a5a89b424095dbb7f67ff2aeddb277
Size: 6144 bytes
PE timestamp: 2004-03-17 16:34:32 UTC
FileVersion: 1, 08, 05, 00
ProductVersion: 1, 0, 0, 1
OriginalFilename: NVoclock.RC
Version-resource product name: NVidia System Utility Driver
Version-resource company name: NVidia Corp.
Authenticode status: NotSigned
Signer: none
Certificate thumbprint: none
Device: \\.\NVR0Dev
PDB: D:\Bld\nvsu1.0\tools\wpt\sys\objfre_wnet_AMD64\amd64\nvoclk64.pdb
Test image: locally test-signed copy of this file, SHA-256 9320f697ff7b99aba660b122c29a032aaccd827395a97f6d09bac41bd89b5fa3, 7544 bytes. The test certificate subject is CN=ioctlance-lab, thumbprint 7BF17BAB52FAA848C2BF4D6B47A71F4D2DDE2694, and is not an NVIDIA signature.

CVE:

Description:
nvoclock.sys in NVIDIA Corporation NVidia System Utility Driver FileVersion 1, 08, 05, 00 (SHA-256 2203bd4731a8fdc2a1c60e975fd79fd5985369e98a117df7ee43c528d3c85958) passes a caller-supplied I/O port and a caller-supplied dword to OUT on IOCTL 0x9C40A484 command 0x13 (METHOD_BUFFERED) when that port is not greater than 0xFFFF and the width is 4, does not compare the port with an address stored by the driver, and does not call HalTranslateBusAddress or MmMapIoSpace, which allows a local user to read and write any I/O port from 0 through 0xFFFF via \\.\NVR0Dev. Command 0x12 on the same IOCTL reads a dword from that caller-supplied port. The Authenticode status is NotSigned. VERSIONINFO CompanyName is NVidia Corp., FileDescription and ProductName are NVidia System Utility Driver, FileVersion is 1, 08, 05, 00, ProductVersion is 1, 0, 0, 1, and OriginalFilename is NVoclock.RC. On 2026-11-03, on an isolated Windows 10 Enterprise LTSC 2021 virtual machine (10.0.19044.7807), an administrator loaded a test-signed copy of this file after sc start of the unsigned original returned Windows error 577, and the service started. The running image is SHA-256 9320f697ff7b99aba660b122c29a032aaccd827395a97f6d09bac41bd89b5fa3, 7544 bytes. The test certificate is not an NVIDIA signature. Local account aida-low, a member of Users and not a member of Administrators, opened \\.\NVR0Dev, read port 0x80 as 0x00000000, wrote 0x0000005A and read 0x0000005A back, then wrote 0x000000A5 and read 0x000000A5 back, then wrote 0x00000000 and read 0x00000000 back. The test did not show a crash. The device security descriptor was O:BAG:SYD:(A;;0x1201bf;;;WD)(A;;FA;;;SY)(A;;FA;;;BA)(A;;0x1200a9;;;RC)S:AI(ML;;NW;;;LW). Everyone (WD) has access mask 0x1201bf, which includes FILE_READ_DATA and FILE_WRITE_DATA.

Suggested fix: on IOCTL 0x9C40A484 commands 0x13 and 0x12, do not pass a caller-supplied port to OUT or IN; use only a port chosen inside the driver, and limit access to \\.\NVR0Dev to Administrators and SYSTEM.
