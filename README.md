# udp-pipeline

## About Folders

header：header-file in dada-format 

pipeline：the main process in GPU

udp：merge udp-package to dada-data

src和include：C source & header files dependency.

## Revise /scripts/pipeline.sh (if needed)

       1. ringbuffer
       
       raw（raw inputs），bmf（after beamforming），zoo（after zoom-fft）
       
       1）set ringbuffer keywords： a000， b000, c000
       
       2）set thread numbers
       
       3）set ringbffer size： bufsz_raw, bufsz_bmf, bufsz_zoo
       
       compute sizes of all ringbuffers
       
       Input（a000）：npkt×nelement×npol×pkt_data×sizeof(int8_t)
       
       beamformr output power（b000）： npkt×pkt_nsamp×beam×bf_nchan×sizeof(float)/naverage_bf
       
       zoomfft output power（c000）：zoom_nsamp×zoom_nchan×bf_nchan×sizeof（float）/naverage_zoom

       
       4）creat 3 ringbuffers based on pre-set parameters. 
        
       2. data storage
       
       set storage folders：dir_raw, dir_bmf, dir_zoo
       
       active data-storing via dada_dbdisk：dada_dbdisk -D 'save_dir' -k 'ringbuffer' -W
       
       3. lauch pipeline and udp2db
       ../build/pipeline/pipeline_dada_beamform -i $key_raw -o $key_bmf -n $nreader_raw -g 0
       ../build/udp/udp2db -k $key_raw -i 10.11.4.54 -p 12345 -f ../header/512MHz_1ant1pol_4096B.header -m 56400
       
       4. In the end close data-storing，and clear ringbuffers.
       
## Run the pipeline
       bash pipeline.sh
       udpgen -i 10.11.4.54 -p 12346 -I 10.11.4.54 -P 12345 -r 10   （revise ip & port，-i & -p mean transmission-end，-I & -P mean recieving-end）

## Contact
       Wang,Yu         ywang@zhejianglab.com
