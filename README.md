# kaldi_carbonate
Run the kaldi singularity container on IU's carbonate system

It requires a couple of things:
1) the singularity container provided by Fraunhofer must be on carbonate
2) the SSH keypair agreement must be signed for IU's HPC and a keypair with
   passphrase must be created & installed

The syntax for the command is:
````
usage: kaldi_carbonate.py [-h] [--debug] [--quiet] [--nocleanup] config input kaldi_transcript_json kaldi_transcript_txt amp_transcript_json

Run the kaldi singularity container on carbonate and retrieve the results

positional arguments:
  config                Configuration file
  input                 input audio file
  kaldi_transcript_json
                        Kaldi JSON output
  kaldi_transcript_txt  Kalid TXT output
  amp_transcript_json   AMP JSON output

optional arguments:
  -h, --help            show this help message and exit
  --debug               Turn on debugging
  --quiet               Turn off output
  --nocleanup           Don't clean up the remote directories

````

When this is run, a working directory is created for this particular instance and the input file copied to it.  A job file is created and submitted to the slurm queueing system that will start the singularity container with the right parameters AND create a finished file that has the return code of the container.   Then the program waits for the finished file to appear.  When it does, the files are copied from the remote system to the destinations specified on the command line.

A 8-hour mp3 file took a total of 10 minutes to run, but 6m of that was transferring the content to the server (I'm on a VPN).  Despite the performance gains, it will not always be faster than the local kaldi:  since Carbonate has few GPUs and they're shared amongst all researchers on campus, the kaldi job may be held until other jobs in the queue are processed.  I have seen delays of more than an hour during testing.

