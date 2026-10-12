Steam games in the Omarchy menu
===============================

Creates an Omarchy menu entry for every installed Steam game and keeps the
list up to date as games are installed or uninstalled. If Steam makes its own
shortcut for a game, it is removed so the game isn't listed twice.

  steam-launcher-sync           Script that writes the menu entries
  steam-launcher-sync.path      Watches the Steam library and menu folders
  steam-launcher-sync.service   Runs the script (on changes and at login)


Install on a new machine
------------------------

1. Copy the files into place (run from this folder):

     mkdir -p ~/.local/bin ~/.config/systemd/user
     cp steam-launcher-sync ~/.local/bin/
     chmod +x ~/.local/bin/steam-launcher-sync
     cp steam-launcher-sync.path steam-launcher-sync.service ~/.config/systemd/user/

2. Edit ~/.config/systemd/user/steam-launcher-sync.path so there is one
   PathChanged= line per Steam library's "steamapps" folder. The second line
   points at this drive as it was mounted on the original machine
   (/run/media/ballad/HIGSBY/...). Fix it if the username or mount point is
   different. Library paths are listed in:

     ~/.local/share/Steam/steamapps/libraryfolders.vdf

   Keep the last PathChanged= line (~/.local/share/applications). It catches
   shortcuts Steam adds after an install so duplicates get removed.

3. Turn it on:

     systemctl --user daemon-reload
     systemctl --user enable --now steam-launcher-sync.path
     systemctl --user enable steam-launcher-sync.service
     steam-launcher-sync

Requirements: Steam (native, not Flatpak) and ImageMagick ("magick") for icons.

Logs:  journalctl --user -u steam-launcher-sync
