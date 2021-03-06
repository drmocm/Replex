# REPLEX #

Replex was created to remultiplex transport stream (TS) data taken from a DVB
source. The result is supposed to be a program stream (PS) that can be
used to be burned to a DVD (with dvdauthor).
Replex can also remultiplex other PSs and AVIs with MPEG2 content.

    usage: ./replex [options] <input files>
 
    options:
      --help,             -h            :  print help message

      --audio_pid,        -a <integer>  :  audio PID for TS stream (also used for PS id, default 0xc0)
      --ac3_id,           -c <integer>  :  ID of AC3 audio for demux (also used for PS id, i.e. 0x80)
      --video_delay,      -d <integer>  :  video delay in ms
      --audio_delay,      -e <integer>  :  audio delay in ms
      --ignore_PTS,       -f            :  ignore all PTS information of original
      --larger_buffer     -g <integer>  :  video buffer in MB
      --input_stream,     -i <string>   :  set input stream type (string = TS(default), PS, AVI)
      --allow_jump,       -j            :  allow jump in the PTS and try repair
      --keep_PTS,         -k            :  keep and don't correct PTS information of original
      --min_jump,         -l <integer>  :  don't try to fix jumps in PTS larger than <int> but treat them as a cut (default 100ms)
      --of,               -o <filename> :  set output file
      --fillzero          -p            :  fill audio frames with zeros (only MPEG AUDIO)
      --max_overflow      -q <integer>  :  max_number of overflows allowed (default: 100, 0=no restriction)
      --scan,             -s            :  scan for streams
      --type,             -t <string>   :  set output type (string = MPEG2, DVD, HDTV)
      --video_pid,        -v <integer>  :  video PID for TS stream (also used for PS id, default 0xe0)
      --vdr,              -x            :  handle AC3 for vdr input file
      --analyze,          -y <integer>  :  analyze (0=video,1=audio, 2=both)
      --demux,            -z            :  demux only (-o is basename)

A typical call would be
    
	replex -t DVD -o mynewps.mpg myoldts.ts

Replex can guess the PIDs of your audio and video streams, but
especially if you have more than one audio stream you should use the
-v and -a or -c options. The -a and -c options can be used more than
once to create multiple audio tracks. Use the -s option to find out 
about the PIDs in your file

The -k option means that replex tries to keep the original PTS spacing,
which can be helpful in case of corrupt streams. Replex will ignore
missing frames and just keep the PTS intervals between the frame it
can find as given by the original file.

The opposite is the -f option, which just ignores all PTS information
from t]he original and creates the PTS according to the frames that are
found.

The -j option lets replex jump over PTS discontiniuties like the ones
you get from cutting an MPEG file. With -l you can set the minimum time 
a jump has to have to get treated as cut. The default for that is 100ms.
If you have video jumps without any audio jumps and they are larger than 
100ms (usually > 2 frames) you can try increasing the min_jump setting.
If you set it too high and you have real jumps, e.g. audio and video 
jumps from a cut, they may not get fixed correctly.


The -g option can be helpful if you get ringbuffer overflows, it increases
the video buffer size. Default is 6MB.

