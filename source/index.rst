Welcome to EasyCO's documentation!
======================================

**EasyCO** is a learning-driven platform for solving Combinatorial Optimization (CO) problems. We aim to provide the CO community with solvers that are easy-to-use, flexible, and broadly
applicable.

pipline:

.. figure:: ./_static/pipline.png
    :alt: pipline
    :align: center
    :width: 100%

🚀Features
--------------

- **Extensive Coverage**: EasyCO supports the solution generation for 55 common CO problems. This broad coverage is complemented by a comprehensive suite of benchmark datasets to facilitate fair and robust comparisons. To the best of our knowledge, EasyCO is the first platform to achieve coverage of over 50 problems.
- **Flexible Composition**: EasyCO is built upon a unified optimization pipeline, where each neural solver is decomposed into interchangeable modules. This modular framework allows the flexible composition of components from different solvers, thereby facilitating the rapid development of novel and tailored solutions.
- **Intuitive GUI**: We provide a user-friendly Graphical User Interface (GUI) that enables a full code-free workflow from experimental design to result analysis. This significantly lowers the technical barrier and accelerates the research process.

🌟Supported tasks
-------------------

.. list-table:: **Supported Tasks**
   :header-rows: 1
   :widths: 15 50 35

   * - Problem Category
     - Description
     - Typical Problems
     
   * - Routing Problems
     - Find one or more paths in a graph to minimize cost or maximize reward under constraints.
     - TSP, ATSP, PCTSP, CVRP, OP, SOP
   * - Scheduling Problems
     - Assign start times or order to tasks under resource/time constraints to optimize goals.
     - FFSP, RCPSP, SMTWTP
   * - Packing Problems
     - Select items under capacity limits to maximize value or minimize container usage.
     - KP, MKP, BPP
   * - Other Graph Problems
     - Match items or select subsets under structural constraints to optimize objective.
     - MIS



.. list-table:: **Solvers**
   :header-rows: 1
   :widths: 15 85

   * - Problem
     - Solvers
     
   * - TSP
     - 
   * - CVRP
     - 
   * - OP
     - 
   * - ATSP
     - 
   * - PCTSP
     - 
   * - SOP
     - 



🎉Supported methods
----------------------

EasyCO supports the following classical methods:

.. list-table:: Supported Classical Methods
   :header-rows: 1
   :widths: 15 50 35

   * - Method
     - Paper
     - code
   * - LKH-3
     - An effective implementation of the Lin-Kernighan traveling salesman heuristic (European Journal of Operational Research, 2000)
     - http://akira.ruc.dk/~keld/research/LKH-3/
   * - EAX
     - A Powerful Genetic Algorithm Using Edge Assembly Crossover for the Traveling Salesman Problem (INFORMS Journal on Computing, 2013)
     - https://github.com/nagata-yuichi/GA-EAX
   * - HGS
     - Hybrid genetic search for the CVRP: Open-source implementation and SWAP* neighborhood (Computers & Operations Research, 2022)
     - https://github.com/vidalt/HGS-CVRP
   * - OR-Tools
     - https://ai.googleblog.com/2019/09/or-tools-now-supports-integer.html
     - \
   * - Concorde
     - Concorde TSP solver (2006)
     - https://github.com/jvkersch/pyconcorde
   * - PyVRP
     - 
     - 

EasyCO supports the following NCO methods:

.. list-table:: Supported NCO Methods
   :header-rows: 1
   :widths: 15 85

   * - Method
     - Paper
   * - AM
     - `Attention, Learn To Solve Routing Problems! <https://arxiv.org/pdf/1803.08475>`_ (ICLR, 2019)
   * - DACT
     - `Learning to Iteratively Solve Routing Problems with Dual-Aspect Collaborative Transformer <https://arxiv.org/pdf/2110.02544>`_ (NeurIPS, 2021)
   * - DeepACO
     - `DeepACO: Neural-enhanced Ant Systems for Combinatorial Optimization <https://arxiv.org/pdf/2309.14032>`_ (NeurIPS, 2023)
   * - DIFUSCO
     - `Difusco: Graph-based diffusion solvers for combinatorial optimization <https://proceedings.neurips.cc/paper_files/paper/2023/hash/0ba520d93c3df592c83a611961314c98-Abstract-Conference.html>`_ (NeurIPS 2023)
   * - DPN
     - `DPN: decoupling partition and navigation for neural solvers of min-max vehicle routing problems <http://arxiv.org/abs/2405.17272>`_ (ICML, 2024)
   * - ELG
     - `Towards generalizable neural solvers for vehicle routing problems via ensemble with transferrable local policy <https://arxiv.org/abs/2308.14104>`_ (IJCAI, 2024)
   * - GLOP
     - 
   * - H-TSP
     - `H-TSP: Hierarchically Solving the Large-Scale Traveling Salesman Problem <https://www.microsoft.com/en-us/research/publication/h-tsp-hierarchically-solving-the-large-scale-traveling-salesman-problem/>`_ (AAAI 2023)
   * - ICAM
     - `Instance-Conditioned Adaptation for Large-scale Generalization of Neural Combinatorial Optimization <http://arxiv.org/abs/2405.01906>`_ (2024)
   * - INViT
     - `Invit: A generalizable routing problem solver with invariant nested view transformer <https://arxiv.org/abs/2402.02317>`_ (ICML, 2024)
   * - L2S
     - `Deep Reinforcement Learning Guided Improvement Heuristic for Job Shop Scheduling <https://arxiv.org/pdf/2211.10936>`_ (ICLR 2024)
   * - LEHD
     - `Neural combinatorial optimization with heavy decoder: Toward large scale generalization <https://proceedings.neurips.cc/paper_files/paper/2023/hash/1c10d0c087c14689628124bbc8fa69f6-Abstract-Conference.html>`_ (NeurIPS, 2023)
   * - LIH
     - `Learning Improvement Heuristics for Solving Routing Problems <https://ieeexplore.ieee.org/document/9393606>`_ (TNNLS, 2021)
   * - MatNet
     - `Matrix Encoding Networks for Neural Combinatorial Optimization <https://proceedings.neurips.cc/paper/2021/hash/29539ed932d32f1c56324cded92c07c2-Abstract.html>`_ (NeurIPS, 2021)
   * - NLNS
     - `Neural large neighborhood search for routing problems <https://www.sciencedirect.com/science/article/pii/S0004370222001266>`_ (Artificial Intelligence, 2022)
   * - OMNI
     - `Towards Omni-generalizable Neural Methods for Vehicle Routing Problems <https://proceedings.mlr.press/v202/zhou23o/zhou23o.pdf>`_ (ICML, 2023)
   * - POMO
     - `POMO: Policy Optimization with Multiple Optima for Reinforcement Learning <https://proceedings.neurips.cc/paper/2020/hash/f231f2107df69eab0a3862d50018a9b2-Abstract.html>`_ (NeurIPS, 2020)
   * - T2T
     - `T2t: From distribution learning in training to gradient search in testing for combinatorial optimization <https://proceedings.neurips.cc/paper_files/paper/2023/hash/9c93b3cd3bc60c0fe7b0c2d74a2da966-Abstract-Conference.html>`_ (NeurIPS, 2023)
   * - UDC
     - `UDC: a unified neural divide-and-conquer framework for large-scale combinatorial optimization problem <http://arxiv.org/abs/2407.00312>`_ (NeurIPS, 2024)

.. note::
    1111

.. tip::
    2222


🧭Navigation
---------------
.. toctree::
    :maxdepth: 1
    :caption: Getting Started

    getting_started/installation
    getting_started/quickstart
    getting_started/online_demo
    getting_started/gui

.. toctree::
    :maxdepth: 1
    :caption: Developer Documentation

    developer_doc/overview
    developer_doc/data
    developer_doc/exact_solvers
    developer_doc/neural_solvers
    developer_doc/phases
    developer_doc/setting

.. toctree::
    :maxdepth: 1
    :caption: Task

    tasks/atsp
    tasks/bpp
    tasks/cvrp
    tasks/ffsp
    tasks/kp
    tasks/mis
    tasks/mkp
    tasks/op
    tasks/pctsp
    tasks/rcpsp
    tasks/smtwtp
    tasks/sop
    tasks/tsp

.. toctree::
    :maxdepth: 1
    :caption: Method

    methods/am
    methods/bq
    methods/dact
    methods/deepaco
    methods/difusco
    methods/dpn
    methods/elg
    methods/glop
    methods/htsp
    methods/icam
    methods/invit
    methods/l2s
    methods/lehd
    methods/LIH
    methods/matnet
    methods/MTPOMO
    methods/MVMoE
    methods/nlns
    methods/omni
    methods/pointerformer
    methods/pomo
    methods/t2t
    methods/udc

🤝About EasyNCO
---------------------



📄Citation
-------------------

.. code-block:: bibtex

    @inproceedings{ye2023deepaco,
      title={DeepACO: Neural-enhanced Ant Systems for Combinatorial Optimization},
      author={Ye, Haoran and Wang, Jiarui and Cao, Zhiguang and Liang, Helan and Li, Yong},
      booktitle={Advances in Neural Information Processing Systems},
      year={2023}
    }
