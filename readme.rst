File Header
There is only one file header in a binary file. The file header consists of the following information.

Cookie Two byte characters, AG, that indicate the file is in the Agilent Binary Data file format.
Version Two bytes that represent the file version.
File Size A 32-bit integer that is the number of bytes that are in the file.
Number of Waveforms A 32-bit integer that is the number of waveforms that are stored in the file.

Waveform Header
It is possible to store more than one waveform in the file, and each waveform stored will have a waveform header. When using segmented memory, each segment is treated as a separate waveform. The waveform header contains information about the type of waveform data that is stored following the waveform data header.
Header Size A 32-bit integer that is the number of bytes in the header. Waveform Type A 32-bit integer that is the type of waveform stored in the file:

0 = Unknown.
1 = Normal.
2 = Peak Detect.
3 = Average.
4 = Not used in InfiniiVision oscilloscopes. 5 = Not used in InfiniiVision oscilloscopes. 6 = Logic.

Number of Waveform Buffers A 32-bit integer that is the number of waveform buffers required to read the data.
Points A 32-bit integer that is the number of waveform points in the data.
Count A 32-bit integer that is the number of hits at each time bucket in the waveform record when the waveform was created using an acquisition mode like averaging. For example, when averaging, a count of four would mean every waveform data point in the waveform record has been averaged at least four times. The default value is 0.
X Display Range A 32-bit float that is the X-axis duration of the waveform that is displayed. For time domain waveforms, it is the duration of time across the display. If the value is zero then no data has been acquired.
X Display Origin A 64-bit double that is the X-axis value at the left edge of the display. For time domain waveforms, it is the time at the start of the display. This value is treated as a double precision 64-bit floating point number. If the value is zero then no data has been acquired.
X Increment A 64-bit double that is the duration between data points on the X axis. For time domain waveforms, this is the time between points. If the value is zero then no data has been acquired.
X Origin A 64-bit double that is the X-axis value of the first data point in the data record. For time domain waveforms, it is the time of the first point. This value is treated as a double precision 64-bit floating point number. If the value is zero then no data has been acquired.
X Units A 32-bit integer that identifies the unit of measure for X values in the acquired data:

0 = Unknown.
1 = Volts.
2 = Seconds.
3 = Constant.
4 = Amps.
5=dB.
6=Hz.

Y Units A 32-bit integer that identifies the unit of measure for Y values in the acquired data. The possible values are listed above under “X Units”.
Date A 16-byte character array, left blank in InfiniiVision oscilloscopes.
Time A 16-byte character array, left blank in the InfiniiVision oscilloscopes.
Waveform Label A 16 byte character array that contains the label assigned to the waveform.
Time Tags A 64-bit double, only used when saving multiple segments (requires segmented memory option). This is the time (in seconds) since the first trigger.
Segment Index A 32-bit unsigned integer. This is the segment number. Only used when saving multiple segments.

Waveform Data Header
A waveform may have more than one data set. Each waveform data set will have a waveform data header. The waveform data header consists of information about the waveform data set. This header is stored immediately before the data set.
Waveform Data Header Size A 32-bit integer that is the size of the waveform data header.
Buffer Type A 16-bit short that is the type of waveform data stored in the file:

0 = Unknown data.
1 = Normal 32-bit float data.
2 = Maximum float data.
3 = Minimum float data.
4 = Not used in InfiniiVision oscilloscopes.
5 = Not used in InfiniiVision oscilloscopes.
6 = Digital unsigned 8-bit char data (for digital channels).
Bytes Per Point A 16-bit short that is the number of bytes per data point.
Buffer Size A 32-bit integer that is the size of the buffer required to hold the data points.
