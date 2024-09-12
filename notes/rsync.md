-a, --archive
       This is equivalent to -rlptgoD. It is a quick way  of  saying  you  want  recursion  and  want  to
       preserve  almost  everything  (with -H being a notable omission).  The only exception to the above
       equivalence is when --files-from is specified, in which case -r is not implied.

       Note that -a does not preserve hardlinks, because finding multiply-linked files is expensive.  You
       must separately specify -H.
       
-z, --compress
       With  this  option, rsync compresses the file data as it is sent to the destination machine, which
       reduces the amount of data being transmitted -- something that is useful over a slow connection.

       Note that this option typically achieves better compression ratios than can be achieved by using a
       compressing  remote  shell  or  a compressing transport because it takes advantage of the implicit
       information in the matching data blocks that are not explicitly sent over the connection.

       See the --skip-compress option for the default list of file suffixes that will not be compressed.
       
-h, --human-readable

--progress

--dry-run

--delete

> doesn't create Books directory
rsync -avh --progress --delete --dry-run /media/asuka/Books/ 192.168.1.102:Books #Sylvanas Books

> creates Books directory, ie, it creates /home/vicfred/data/Books
rsync -avh --progress --delete --dry-run /home/vicfred/ayaka/Books /home/vicfred/data

> see https://gist.github.com/KartikTalwar/4393116
rsync -aHAXxv --numeric-ids --delete --progress -e "ssh -T -c arcfour -o Compression=no -x" user@<source>:<source_dir> <dest_dir>

^ consider turning off compression


========
rsync -avzh --progress --delete /media/tomiko/Anime/ /media/ayaka/Anime


======== KOBO
mount -t vfat /dev/sda /home/vicfred/ayaka/ -o rw,uid=1000,gid=1000
rsync -avzh --progress --delete --no-perms /home/vicfred/data/ebooks/math/ /home/vicfred/kobo/math
rsync -avzh --progress --delete --no-perms /home/vicfred/data/ebooks/compsci/ /home/vicfred/kobo/compsci