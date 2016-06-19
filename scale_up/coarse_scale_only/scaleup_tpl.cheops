from __future__ import division

tpl_str='''#!/bin/bash -l 
#SBATCH --nodes={{ nodes }}
#SBATCH --ntasks-per-node=12
#SBATCH --ntasks={{ procs  }}
#SBATCH --cpus-per-task=1
#SBATCH --mem=23gb 
#SBATCH --time=00:30:00 
#SBATCH --account=UniKoeln 
#SBATCH --partition=mpi
#SBATCH --constraint=[inca12*{{ nodes }}]
#SBATCH --exclusive
#SBATCH -o /scratch/rmilk/multiscale-build-icc/dune-multiscale/refine_{{ nodes }}.out
#SBATCH -e /scratch/rmilk/multiscale-build-icc/dune-multiscale/refine_{{ nodes }}.err
#SBATCH --mail-user=rmilk@uni-koeln.de
#SBATCH --mail-type=end 

source $HOME/.bashrc
source $HOME/.modules.icc
#for some reasong the above DOES NOT set below vars correctly
export PATH=/home/rmilk/.local/bin:/home/rmilk/local/bin:$PATH
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/rmilk/local/lib
cd /scratch/rmilk/multiscale-build-icc/dune-multiscale/

BIN=/scratch/rmilk/multiscale-build-icc/dune-multiscale/elliptic_msfem
OPT="/scratch/rmilk/multiscale-build-icc/dune-multiscale/refine_{{ nodes }}.ini"
srun -n {{ procs }} -N {{ nodes }} $BIN ${OPT}
push_notify "cheops refine_{{ nodes }} done"
'''

from jinja2 import Template
from math import pow, ceil

tpl=Template(tpl_str)

def output_run(i):
  procs = int(pow(3,i))
  nodes = int(ceil(procs / 12))
  #print('nodes {} - procs {}'.format(nodes, procs))
  fn = 'node_{}.submit'.format(nodes)
  open(fn, 'wt').write(tpl.render(**locals()))
  print('sbatch {}'.format(fn))


for i in range(4,7):
  output_run(i)
#output_run(1)
