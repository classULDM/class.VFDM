
CLASS.VFDM: Cosmic Linear Anisotropy Solving System for an Ultralight Vector Field Dark Matter model
==============================================

Authors: Tomás Ferreira Chase

In collaboration with M. Leizerovich, D. López Nacir, S. Landau.

Overview
--------

This is a modification of CLASS v3.2.5 that replaces (or complements) the cold
dark matter component with an ultralight Vector Field Dark Matter (VFDM)
candidate: a massive, real, minimally-coupled Proca field. The code solves
the full background evolution of the vector field and its linear perturbations
(scalar and tensor sectors), and is able to produce CMB angular power
spectra, the linear matter power spectrum, and the tensor (gravitational wave)
sector sourced by the anisotropic stress of the vector field.

The physics, conventions and equations of motion follow the treatment presented in our companion paper:

- T. Ferreira Chase, D. López Nacir; arxiv:2311.09373
- T. Ferreira Chase, M. Leizerovich, D. López Nacir, S. Landau; arXiv:2408.12052.
- T. Ferreira Chase, D. López Nacir; 

Please cite arXiv:2408.12052 (in addition to the standard CLASS references) if you
use this code in a publication.

What is new with respect to standard CLASS
-------------------------------------------

- New background species `vf` (vector field) with its own energy density,
  pressure, shear and anisotropic stress, including the option of a
  homogeneous-but-anisotropic (Bianchi-I) background.
- Modified scalar and tensor perturbation equations in a Bianchi-I background.
- Gravitational wave sector sourced by the VFDM anisotropic stress.

Main VFDM input parameters
--------------------------

The new parameters are declared in the `.ini` file, together with the usual
CLASS ones. The most relevant ones are:

- `Omega_vf`         : present-day density fraction of the vector field.
                       Replaces (fully or partially) `Omega_cdm`. The budget
                       equation is closed automatically by shooting.
- `vf_parameters`    : comma-separated list. The first entry is the vector
                       field mass in eV (e.g. `1.e-22`). The remaining entries
                       are used by the shooting / initial-condition machinery.
                       Example: `vf_parameters = 1.e-22, 1.e-16, 1.e-30, 0.01`.
- `vector_background_mode` : `frw` or `bianchi`. Selects whether the
                             background is assumed isotropic (FRW) or of
                             Bianchi-I type, the latter being the physically
                             consistent choice for a coherent vector field.
- `svt_coupling`     : `yes` / `no`. Switches on/off the scalar-vector-tensor
                       coupling in the perturbation equations.
- `gamma_Ak`         : angle (in rad) between the background vector
                       orientation and the Fourier mode `k`. 


Python / notebooks
------------------

In [notebooks/Python_wrapper/](notebooks/Python_wrapper/) we include the Jupyter
notebooks used to produce the plots of the papers associated with this code,
as well as further diagnostics. 

Notebooks provided:

- `Background.ipynb`              — background evolution of the Proca field.
- `one_k.ipynb`                   — scalar perturbations at a single `k`.
- `one_k_tensor.ipynb`            — tensor perturbations and GW sourcing.
- `one_k_gamma_sweep.ipynb`       — dependence of perturbations on `gamma_Ak`.
- `Pk_parametrization.ipynb`,     — matter power spectrum parameterization.
  `Pk_theta_sweep.ipynb`,         — dependence of matter power spectrum on `gamma_Ak`.
  `Pk_errors.ipynb`               — comparison of matter power spectrum with LCDM and a scalar field model.
- `Tensor_power_spectrum.ipynb`   — tensor power spectrum.
- `Velocity_invariant_transfer.ipynb` — velocity-invariant transfer function.

The file class_env.yml lists the minimum requirements to create a conda environment for running CLASS.VFDM and the companion notebooks (tested on Ubuntu). Create it with conda env create -f class_env.yml and activate with conda activate class_env.


Below is the original CLASS documentation, which should be followed for
instructions about installation and compilation of the code.

==============================================

CLASS: Cosmic Linear Anisotropy Solving System
==============================================

Authors: Julien Lesgourgues, Thomas Tram, Nils Schoeneberg

with several major inputs from other people, especially Benjamin
Audren, Simon Prunet, Jesus Torrado, Miguel Zumalacarregui, Francesco
Montanari, Deanna Hooper, Samuel Brieden, Daniel Meinert, Matteo Lucca, etc.

For download and information, see http://class-code.net

Compiling CLASS and getting started
-----------------------------------

(the information below can also be found on the webpage, just below
the download button)

Download the code from the webpage and unpack the archive (tar -zxvf
class_vx.y.z.tar.gz), or clone it from
https://github.com/lesgourg/class_public. Go to the class directory
(cd class/ or class_public/ or class_vx.y.z/) and compile (make clean;
make class). You can usually speed up compilation with the option -j:
make -j class. If the first compilation attempt fails, you may need to
open the Makefile and adapt the name of the compiler (default: gcc),
of the optimization flag (default: -O4 -ffast-math) and of the OpenMP
flag (default: -fopenmp; this flag is facultative, you are free to
compile without OpenMP if you don't want parallel execution; note that
you need the version 4.2 or higher of gcc to be able to compile with
-fopenmp). Many more details on the CLASS compilation are given on the
wiki page

https://github.com/lesgourg/class_public/wiki/Installation

(in particular, for compiling on Mac >= 10.9 despite of the clang
incompatibility with OpenMP).

To check that the code runs, type:

    ./class explanatory.ini

The explanatory.ini file is THE reference input file, containing and
explaining the use of all possible input parameters. We recommend to
read it, to keep it unchanged (for future reference), and to create
for your own purposes some shorter input files, containing only the
input lines which are useful for you. Input files must have a *.ini
extension. We provide an example of an input file containing a
selection of the most used parameters, default.ini, that you may use as a
starting point.

If you want to play with the precision/speed of the code, you can use
one of the provided precision files (e.g. cl_permille.pre) or modify
one of them, and run with two input files, for instance:

    ./class test.ini cl_permille.pre

The files *.pre are suppposed to specify the precision parameters for
which you don't want to keep default values. If you find it more
convenient, you can pass these precision parameter values in your *.ini
file instead of an additional *.pre file.

The automatically-generated documentation is located in

    doc/manual/html/index.html
    doc/manual/CLASS_manual.pdf

On top of that, if you wish to modify the code, you will find lots of
comments directly in the files.

Python
------

To use CLASS from python, or ipython notebooks, or from the Monte
Python parameter extraction code, you need to compile not only the
code, but also its python wrapper. This can be done by typing just
'make' instead of 'make class' (or for speeding up: 'make -j'). More
details on the wrapper and its compilation are found on the wiki page

https://github.com/lesgourg/class_public/wiki

Plotting utility
----------------

Since version 2.3, the package includes an improved plotting script
called CPU.py (Class Plotting Utility), written by Benjamin Audren and
Jesus Torrado. It can plot the Cl's, the P(k) or any other CLASS
output, for one or several models, as well as their ratio or percentage
difference. The syntax and list of available options is obtained by
typing 'pyhton CPU.py -h'. There is a similar script for MATLAB,
written by Thomas Tram. To use it, once in MATLAB, type 'help
plot_CLASS_output.m'

Developing the code
--------------------

If you want to develop the code, we suggest that you download it from
the github webpage

https://github.com/lesgourg/class_public

rather than from class-code.net. Then you will enjoy all the feature
of git repositories. You can even develop your own branch and get it
merged to the public distribution. For related instructions, check

https://github.com/lesgourg/class_public/wiki/Public-Contributing

Using the code
--------------

You can use CLASS freely, provided that in your publications, you cite
at least the paper `CLASS II: Approximation schemes <http://arxiv.org/abs/1104.2933>`. Feel free to cite more CLASS papers!

Support
-------

To get support, please open a new issue on the

https://github.com/lesgourg/class_public

webpage!
