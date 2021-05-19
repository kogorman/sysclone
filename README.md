# README for cyclone

## Description
The cyclone project is a BASH shell tool for backing up and replicating all disks in a computer system.  It is mostly a BASH frontend for the partclone family of programs, and was develoed on the Ubuntu flavor of Linux operating systems.  It may be useable on others.

Its primary use case is to back up all partitions of all disk drives on a computer, while running Linux from an external drive, with backups going to that same or an additional external drive. It expects there to be a directory (by default a mount point) that contains backups organized by computer name and date of backup, so multiple computers can have multiple backups on the same target drive.

Copies of the current clone and restore scripts are copied to the backup medium, to guard against possible future incompatible changes, and to make it easier to restore to a system that does not already have them.  The scripts are small enough that this seems reasonable.

The code was developed on an Ubuntu system, with partclone installed.  It relies on a number of commands which are normally present on most or all Linux systems, but a few may be more rare, such as `udevadm`, which expects the UDEV device management subsystem.

## Major Options

Clone targets to be backed up or restored can be selected by default, by drive, or by partition, but these variants cannot be mixed in the same operation.

Drives and partitions can be added to a current backup

Drives and partitions can be renamed during either the clone or the restore step.  This is useful to correct for any renaming that occurs because you're booted from an external drive.

The `-h` option prints a man page for the command.  The two commands are sybak and syres.

The clone operation has several expectations (some of which can be overridden by command-line options) that are designed to catch typographic and other errors in the commands.

- The target must contain a text file named `backup.log`
- The system name must already have a directory under `/mnt/target`
- No partition to be backed up is currently mounted
- No backup for the current date and system altready exists
- No Target drive for a restore currently has a partition table at all.


## Requirements
## Examples
### Copy all

> `sybak -N mysys /mnt/target`

This will back up all drives that **do not** currently have partitions mounted, and place the results in a new directory `/mnt/target/mysys` under a subdirectory named by the current date, such as `/mnt/mysys/20200213`.  Since the running system and the target drive necessarily have mounted partitions, they are excluded from the backup.  The backup includes enough information to rebuild the partition tables.

The "-N" option is needed only if this is the first backup to 'mysys'.

`syres mysys /mnt/target`

This will restore all drives and partition from the latest-dated backup on `/mnt/target/mysys`

### Backup or add partitions to a backup

> `sybak -f mysys /mnt/target -p sdg2 -p sdh3``

This will add the /dev/sdg and /dev/sdh to the backup made earlier on the same day.  If no such backup exists, the "-f" option is not needed, and this will begin a new subdirectory.  No drive-level data is backed up by this command.

### Restore partitions to different places

> `syres mysys /mnt/target -p sdg2:sdf2,sdh3:sdf3`

This restores the two partitions that were originally saved as sdg2 and sdh3 onto partitions of /dev/sdf.

> `syres mysys /mnt/target
### Copy a thumb drive / jump stick / USB drive

> `sybak mystick /mnt/target sdh:ISOS`

This will back up everything on /dev/sdh to the directory /mnt/target/mystick in a subdirectory named for the current date.  If this is the first subdirectory, the -N option would also be required.

> `syres mystick /mnt/target ISOS:sdj`

This would restore the latest version of the above backup to the drive currently on /dev/sdj, which must NOT have any partions, preferably not even a partition table.
.
## Limitations

The output must fit within the space remaining on the single target volume.

Only the filesystem and partition types of normal Linux and Windows, DOS and UEFI partition schemes are supported.  This may change if time permits.

RAID arrays are not backed up.

## License and copying

Cyclone is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 2 of the License, or (at your option) any later version published by the Free Software Foundation.

Cyclone is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with Cyclone.  If not, see <https://www.gnu.org/licenses/>.

## Author
> Kevin O'Gorman
> kogorman@gmail.com
