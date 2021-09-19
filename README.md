# Big File Repair Tool

You may have been in the situation where you copied a huge file across a rather slow and sometimes unreliable link, and the copy proved to be corrupt. Usually the corruption is small, like some missing bytes at the end or a few corrupted sectors on a USB stick. What you could do in that case:
1. Just copy over the whole file again, which takes ages.
2. Split up the file, calculate checksums of each chunk, do the same for the original, compare the checksums, copy over the different blocks, and concatenate the repaired blocks.
3. Use this script.

This script basically does option 2, but then more efficiently and as automatically as possible. How to use it:
1. Run the script with the damaged file as argument.
   This produces a `checksums.txt` file. Copy over that file to where the undamaged original is.
2. Run the script with the original file as argument.
   This compares blocks of the file with the values in `checksums.txt`. If a block differs, it extracts it from the original file. The extracted blocks all have file names like `BLOCK_100`.
   It also prints the command to be executed in the next step. The command will also have a truncate argument in case the damaged file somehow is bigger than the original.
3. Copy the `BLOCK_*` files, as well as the command to be executed, back to where the damaged file is. Then run the command with the appropriate file name.
4. The damaged file should now be repaired.

The script has some extra options to override the checksums file name and block size, run it with `-h` for usage instructions.

Currently this requires the GNU flavour of the `dd` command, and it also needs the `md5sums` command to be available. I might update it to also work with BSD style `dd` and `md5` in the future, which will make it easier to use on e.g. Mac OS.

The script relies on MD5 sums which should be adequate for this purpose. If you are extremely unlucky, the file corruption in a certain block could be such that it causes an md5 collision, but this is extremely unlikely. I might update the script to allow using other checksums in the future. Of course, if anyone else feels like doing this, pull requests are welcome.


## License

This is released under a BSD 2-Clause "Simplified" License. See the LICENSE file for details.
As usual, this is provided as-is with no warranties of any kind, use at your own risk.

