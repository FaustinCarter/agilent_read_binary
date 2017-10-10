Agilent oscilloscope binary file converter
==========================================
This repo contains code that reads the '.bin' format included with most
HP/Agilent/Keysight Infiniuum scopes. This is different than the proprietary
'.wav' format but nearly as compact. The file is read in as a dict containing
one key for each channel measured. Each of those keys contains a dict with the
data under the 'y_data' key, unless the data was acquired using the "segments"
feature, in which case the key 'segment_data' contains a list of dicts, each
containing keys for 'time_tag', 'segment_index' and 'y_data'. The general file
format is outlined below. This is largely copied directly from Agilent
documentation, and gives you an idea of what you are looking at when you read
the code. Several of the following attributes are included in the metadata of
the returned dict. Additionally, a datetime object can be created and added to
metadata.

File Header
-----------
There is only one file header in a binary file. The file header consists of the
following information.

Cookie
~~~~~~
Two byte characters, 'AG', that indicate the file is in the Agilent Binary Data
file format.

Version
~~~~~~~
Two bytes that represent the file version.

File Size
~~~~~~~~~
A 32-bit integer that is the number of bytes that are in the file.

Number of Waveforms
~~~~~~~~~~~~~~~~~~~
A 32-bit integer that is the number of waveforms that are stored in the file.

Waveform Header
---------------
It is possible to store more than one waveform in the file, and each waveform
stored will have a waveform header. When using segmented memory, each segment is
treated as a separate waveform. The waveform header contains information about
the type of waveform data that is stored following the waveform data header.

Header Size
~~~~~~~~~~~
A 32-bit integer that is the number of bytes in the header.

Waveform Type
~~~~~~~~~~~~~
A 32-bit integer that is the type of waveform stored in the file:

- 0 = Unknown
- 1 = Normal
- 2 = Peak Detect
- 3 = Average
- 4 = Not used in InfiniiVision oscilloscopes
- 5 = Not used in InfiniiVision oscilloscopes
- 6 = Logic

Number of Waveform Buffers
~~~~~~~~~~~~~~~~~~~~~~~~~~
A 32-bit integer that is the number of waveform buffers required to read the
data.

Points (included in metadata)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
A 32-bit integer that is the number of waveform points in the data.

Count (included in metadata)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~
A 32-bit integer that is the number of hits at each time bucket in the waveform
record when the waveform was created using an acquisition mode like averaging.
For example, when averaging, a count of four would mean every waveform data
point in the waveform record has been averaged at least four times. The default
value is 0.

X Display Range (included in metadata)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
A 32-bit float that is the X-axis duration of the waveform that is displayed.
For time domain waveforms, it is the duration of time across the display. If the
value is zero then no data has been acquired.

X Display Origin (included in metadata)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
A 64-bit double that is the X-axis value at the left edge of the display. For
time domain waveforms, it is the time at the start of the display. This value is
treated as a double precision 64-bit floating point number. If the value is zero
then no data has been acquired.

X Increment (included in metadata)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
A 64-bit double that is the duration between data points on the X axis. For time
domain waveforms, this is the time between points. If the value is zero then no
data has been acquired.

X Origin (included in metadata)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
A 64-bit double that is the X-axis value of the first data point in the data
record. For time domain waveforms, it is the time of the first point. This value
is treated as a double precision 64-bit floating point number. If the value is
zero then no data has been acquired.

X Units (included in metadata)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
A 32-bit integer that identifies the unit of measure for X values in the
acquired data:

- 0 = Unknown
- 1 = Volts
- 2 = Seconds
- 3 = Constant
- 4 = Amps
- 5 = dB
- 6 = Hz

Y Units (included in metadata)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
A 32-bit integer that identifies the unit of measure for Y values in the
acquired data. The possible values are listed above under “X Units”.

Date (included in metadata)
~~~~~~~~~~~~~~~~~~~~~~~~~~~
A 16-byte character array, left blank in InfiniiVision oscilloscopes.

Time (included in metadata)
~~~~~~~~~~~~~~~~~~~~~~~~~~~
A 16-byte character array, left blank in the InfiniiVision oscilloscopes.

Waveform Label (used as dict key)
~~~~~~~~~~~~~~
A 16 byte character array that contains the label assigned to the waveform.

Time Tags (included in metadata of segment_data key)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
A 64-bit double, only used when saving multiple segments (requires segmented
memory option). This is the time (in seconds) since the first trigger.

Segment Index (included in metadata of segment_data key)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
A 32-bit unsigned integer. This is the segment number. Only used when saving
multiple segments.

Waveform Data Header
--------------------
A waveform may have more than one data set. Each waveform data set will have a
waveform data header. The waveform data header consists of information about the
waveform data set. This header is stored immediately before the data set.

Waveform Data Header Size
~~~~~~~~~~~~~~~~~~~~~~~~~
A 32-bit integer that is the size of the waveform data header.

Buffer Type
~~~~~~~~~~~
A 16-bit short that is the type of waveform data stored in the file:

- 0 = Unknown data
- 1 = Normal 32-bit float data
- 2 = Maximum float data
- 3 = Minimum float data
- 4 = Not used in InfiniiVision oscilloscopes
- 5 = Not used in InfiniiVision oscilloscopes
- 6 = Digital unsigned 8-bit char data (for digital channels)

Bytes Per Point
~~~~~~~~~~~~~~~
A 16-bit short that is the number of bytes per data point.

Buffer Size
~~~~~~~~~~~
A 32-bit integer that is the size of the buffer required to hold the data
points.
