## bsub_jupyter_lab
start jupyter lab (python and R) server on LSF farm5 node via bsub.

#### instructions
ssh on farm5, clone repo and run start script with arguments:  
! This requires conda to be activated, see instructions below if not done already !
```
git clone https://github.com/wtsi-hgi/bsub_jupyter_lab.git && cd bsub_jupyter_lab
./bsub_jupyter_lab.sh -g hgi -c 4 -m 50000 -q normal
```

* `-g` is your LSF group
* `-c` is the number of CPUs requested
* `-m` is the memory requested (50000 means 50G)
* `-q` is the LSF queue

The address:port and token of the server will be given in file `jupyter_lab.log` (created in current directory).
That is, wait until you see a line like the following in `jupyter_lab.log` , and paste address in your browser:
```
    Or copy and paste one of these URLs:
        http://node-10-4-1:53074/?token=ea9bba78eb0840154b45acfc90dc9395e66c8d6fbcb2d4be
```

If you are working remotely, you need to be connected via VPN or use [SSH tunneling through the web proxy](https://ssg-confluence.internal.sanger.ac.uk/display/FARM/All+things+SSH#AllthingsSSH-TunnelingthroughtheSSHgateway) to access the node from your web browser. With the latter, you need to forward the port of the host that is running Jupyter Lab on to a port on your local machine. For the example above, open a new terminal session and do    
`ssh -L 53074:node-10-4-1.internal.sanger.ac.uk:53074 your_sanger_username@ssh.sanger.ac.uk` (note the addition of `internal.sanger.ac.uk`), which forwards port 53074 on node-10-4-1 to port 53074 on your localhost, jumping through the SSH gateway. Then you can point your browser to address `127.0.0.1`:  
`http://127.0.0.1:53074/?token=ea9bba78eb0840154b45acfc90dc9395e66c8d6fbcb2d4be`

#### R libraries
2 R versions are currently available in the notebook: R 3.6.1 and R 4.0.0 .  
You can add your own installed libraries (must be compatible with either R 3.6.1 or R 4.0.0) in an R notebook:
```
.libPaths(/lustre/path_to_installed_libraries)
library(your_library_name)
```

#### python libraries
In Jupyter, you can open a terminal and try install packages with a `--target` directory,  e.g.    
    `pip install pandas --target /lustre/path_to_new_pip_libraries`  
and then in a python notebook:
```
    import sys
    sys.path.append('/lustre/path_to_new_pip_libraries')
    import pandas
```

If that doesn’t work for your package because of conda conflicts, contact HGI: you could clone the complete jupyter conda environment and reference the new one in the start script, so that you can alter all conda/pip packages yourself.
 

#### activate hgi conda on farm5
```
ssh farm5-login
gn5@farm5-head2:~$ /software/hgi/installs/anaconda3/bin/conda init bash
<now you must log out and into farm5 again>
<check that conda activate works:>
gn5@farm5-head2:~$ conda activate hgi_base
```
