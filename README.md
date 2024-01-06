# ExternalDriveOffloader
A little Mac OSX daemon to offload local files to an external drive.

This turned into a fun little project where I got to learn a thing or two about Mac OSX daemons, launchd, osascript, and bash.

# What's it for?
I don't like having files spread across multiple machines (desktops, laptops, workstations, etc.), so I'm writing some daemons that can run in the background on those machines; watch for an external drive; and if it's connected move locally stored data to that drive. That way I can have a single storage device for my files.

Ultimately, I want to be able to plug the external storage in and reliably expect that everything I've saved lives on that drive and not elsewhere. I think this will make upgrading my personal computer easier and should make backups simpler as well. I'll tackle the latter as a separate project.

# Workflow
- The daemon watches and waits for a specific drive to be mounted, identified by its UUID.
- When the target drive is mounted:
  - Rsync all of the files stored on the Mac to the attached storage.
  - Remove those files from local storage as they're synced.
  - Update a convenience symlink to point to the external drive so I don't have to go hunting for it.
  - A directory on this external drive is also my Dropbox target directory, so fire up Dropbox if the external drive is available for syncing.
- When the drive is unmounted:
  - Stop Dropbox.
  - Flip the convenience symlink to point to the local machine's storage location.
  - Do periodic sweeps of local storage in case the external drive is connected, but files were accidentally saved locally.

I guarantee there's a better and more efficient way to do this, but this is my first offering. If you have suggestions, please let me know.
