These are the steps to needed to compile the (uncertainty-weighted) Smoothing Algorithm using pybind11 in a virtual environment assuming usage on lxplus.


Source a python 3 environment
```
scl enable rh-python38 bash
```

Create the virtual environment first by running
```
python -m venv my_env
source my_env/bin/activate
```

Install pybind (and numpy for checking output)
```
pip install pybind11
pip install numpy
```

Add the virtual environment to the PYTHONPATH
```
export PYTHONPATH="$WORKINGDIR/my_env/lib/python3.8/site-packages:$PYTHONPATH"
```

From within the 'compiled' directory, compile the h and cpp files to create the pysmoother.so file
```
bash compile.sh
```

Then test the resulting file with the following inputs
```
import numpy as np
import pysmoother
bins = np.array([320.,  360.,  400.,  420.,  440.,  460.,  480.,  500.,  520.,540.,  560.,  580.,  600.,  625.,  650.,  675.,  700.,  730., 760.,  800.,  850.,  900.,  950., 1000., 1050., 1100., 1150.,1200., 1300., 1500., 2000.])
yvals = np.array([0.97990867, 0.9789233 , 0.98536664, 0.98538513, 0.9869879 ,0.9861116 , 0.98888901, 0.98982274, 0.9910679 , 0.99226457,0.99099154, 0.99167056, 0.99575146, 0.99227106, 0.993026  ,0.99161265, 0.99863964, 0.99579265, 0.98869752, 0.99758525,0.99949889, 1.00216327, 0.99948711, 1.0036999 , 0.98140692,1.01842435, 0.99879733, 1.01334406, 1.01597298, 1.01488018])
ywvals = np.array([0.00124149, 0.001038  , 0.00154074, 0.00161407, 0.00169038,0.00178818, 0.00191579, 0.00206513, 0.0022353 , 0.00241739,0.002614  , 0.00283341, 0.00278523, 0.00306559, 0.00338434, 0.00373478, 0.00381412, 0.00425329, 0.00417864, 0.0044108 ,0.0052753 , 0.0062202 , 0.00728632, 0.008531  , 0.00970912,0.01145645, 0.01304903, 0.01150976, 0.01210448, 0.01727745])
p_end = 0.5
smooth = np.zeros(yvals.size)
pysmoother.py_smoothhist(bins, yvals, ywvals, p_end, smooth)

```
The resulting smoothed array should be
```
array([0.97982798, 0.98182129, 0.98491189, 0.98597151, 0.9864844 ,
       0.98858877, 0.98979783, 0.9910943 , 0.9921268 , 0.99265235,
       0.99267244, 0.99353313, 0.99496516, 0.99455058, 0.99402147,
       0.99398405, 0.9970909 , 0.99809717, 0.99852567, 0.99900822,
       0.99884472, 1.00311332, 1.00065816, 0.99936103, 0.99945793,
       1.00161774, 1.00488136, 1.01591817, 1.00566897, 1.00009318])
```
