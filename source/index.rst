Welcome to EasyNCO's documentation!
======================================

EasyCO is a learning-driven platform for solving Combinatorial Optimization (CO) problems. We aim to provide the CO community with solvers that are easy-to-use, flexible, and broadly
applicable.

pipline:

.. figure:: ./_static/pipline.png
    :alt: pipline
    :align: center
    :width: 100%



🚀Features
--------------

- Extensive Coverage: EasyCO supports the solution generation for 55 common CO problems. This broad coverage is complemented by a comprehensive suite of benchmark datasets to facilitate fair and robust comparisons. To the best of our knowledge, EasyCO is the first platform to achieve coverage of over 50 problems.
- Flexible Composition: EasyCO is built upon a unified optimization pipeline, where each neural solver is decomposed into interchangeable modules. This modular framework allows the flexible composition of components from different solvers, thereby facilitating the rapid development of novel and tailored solutions.
- Intuitive GUI: We provide a user-friendly Graphical User Interface (GUI) that enables a full code-free workflow from experimental design to result analysis. This significantly lowers the technical barrier and accelerates the research process.

🎉Supported methods
----------------------

The diverse initializations and iterations in our EasyCO:
+-------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
| Category          | Solvers                                                                                                                                     |
+===================+=============================================================================================================================================+
| Classical Solvers | LKH-3, Concorde, HGS, EAX, PyVRP, OR-Tools.                                                                                                 |
+-------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
| Initialization    | Ptr-Nets, AM, POMO, ELG, ICAM, INViT, Omni_VRP, H-TSP, Pointerformer, DPN, MatNet, LEHD-Greedy, DIFUSCO-Greedy, T2T-Greedy, Fast-T2T-Greedy |
+-------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
| Iteration         | GLOP, UDC, DeepACO, LIH, DACT, NLNS, LEHD-RRC, DIFUSCO-2-OPT, T2T-2-OPT, Fast-T2T-OPT                                                       |
+-------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
|                   | MT-POMO, MVMoE, CaDA                                                                                                                        |
+-------------------+---------------------------------------------------------------------------------------------------------------------------------------------+

EasyNCO supports the following NCO methods:
+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
| Methods         | Paper title                                                                                                                 |
+=================+=============================================================================================================================+
| AM              | Attention, Learn To Solve Routing Problems! (ICLR 2019)                                                                     |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
| POMO            | POMO: Policy Optimization with Multiple Optima for Reinforcement Learning (NeurIPS 2020)                                    |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
| LEHD            | Neural Combinatorial Optimization with Heavy Decoder: Toward Large Scale Generalization (NeurIPS 2023)                      |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
| INViT           | INViT: A Generalizable Routing Problem Solver  with Invariant Nested View Transformer (ICML 2024)                           |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
| ELG             | Towards Generalizable Neural Solvers for Vehicle Routing Problems via Ensemble with Transferrable Local Policy (IJCAI 2024) |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
| DeepACO         | DeepACO: Neural-enhanced Ant Systems for Combinatorial Optimization (NeurIPS 2023)                                          |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
| NLNS            | Neural large neighborhood search for routing problems (Artificial Intelligence, 2022)                                       |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
| 1111            | 1111                                                                                                                        |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
| 1111            | 1111                                                                                                                        |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
| 1111            | 1111                                                                                                                        |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
| 1111            | 1111                                                                                                                        |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------+


.. note::
    1111

.. tip::
    2222


🌟Supported tasks
-------------------

+----------------------------+----------------------------------------+----------------------------------------------------------------------------------------------------------+
| Problem Category           | Typical Problems                       | Description                                                                                              |
+============================+========================================+==========================================================================================================+
| Routing Problems           | TSP, ATSP, PCTSP, CVRP, OP, SOP        | Find one or more paths in a graph to minimize cost or maximize reward under constraints.                 |
+----------------------------+----------------------------------------+----------------------------------------------------------------------------------------------------------+
| Scheduling Problems        | FFSP, RCPSP, SMTWTP                    | Assign start times or order to tasks under resource/time constraints to optimize goals.                  |
+----------------------------+----------------------------------------+----------------------------------------------------------------------------------------------------------+
| Packing Problems           | KP, MKP, BPP                           | Select items under capacity limits to maximize value or minimize container usage.                        |
+----------------------------+----------------------------------------+----------------------------------------------------------------------------------------------------------+
| Other Graph Problems       | MIS                                    | Match items or select subsets under structural constraints to optimize objective.                        |
+----------------------------+----------------------------------------+----------------------------------------------------------------------------------------------------------+


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
    :caption: Overview

    overview/introduction
    overview/platform_structure

.. toctree::
    :maxdepth: 1
    :caption: Developer Documentation

    developer_doc/setting
    developer_doc/data
    developer_doc/exact_solvers
    developer_doc/methods
    developer_doc/phases

.. toctree::
    :maxdepth: 2
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
    methods/invit
    methods/lehd
    methods/LIH
    methods/matnet
    methods/MTPOMO
    methods/MVMoE
    methods/nlns
    methods/pointerformer
    methods/pomo
    methods/t2t



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
