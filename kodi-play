#!/bin/bash

## YouTube Kodi Script [http://github.com/pnd4/kodi-play] 
#by pnd4 
#
# - Portions from "YouTube XBMC Script" by Tom Laermans [tomlaermans.net]. 
#   This script is (also) released into the public domain.
# - Description: Uses Kodi's native JSON-RPC to play YouTube content remotely
#   without need for the webserver.
# - Requires: netcat (tested with GNU Netcat)
# - Usage: kodi-play < URL | YouTube-ID >
# -    ex: kodi-play hABj_mrP-no

## Configure Kodi's RPC details here
KODI_HOST=gen2
KODI_PORT=9090

## Don't touch anything under here

REGEX="^.*((youtu.be\/)|(v\/)|(\/u\/\w\/)|(embed\/)|(watch\?))\??v?=?([^#\&\?]*).*"

ID=$1

if [ "$ID" == "" ];
then
  echo "Syntax $0 <id|url>"
  exit
fi

if [[ $ID =~ $REGEX ]]; then
  ID=${BASH_REMATCH[7]}
fi

# Sends our JSON-RPC request to Kodi, and closes the connection.
function jrpc_req {
    echo -n "$1" EOF | nc -c $KODI_HOST $KODI_PORT;
}

echo -n "Opening video id $ID on $KODI_HOST ... "


jrpc_req '{"jsonrpc": "2.0", 
           "method": "Playlist.Clear", 
           "params":{"playlistid":1}, 
           "id": 0}';

jrpc_req '{"jsonrpc": "2.0", 
           "method": 
           "Playlist.Add", 
           "params":{"playlistid":1, 
           "item" :{ "file" : "plugin://plugin.video.youtube/?action=play_video&videoid='$ID'"}}, 
           "id": 0}';

jrpc_req '{"jsonrpc": "2.0", 
           "method": 
           "Player.Open", 
           "params":{"item":{"playlistid":1, "position" : 0}}, 
           "id": 0}';

echo "Done."
