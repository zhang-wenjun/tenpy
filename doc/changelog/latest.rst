[latest]
========

Release Notes
-------------
The default (stable) git branch was renamed from ``master`` to ``main``.

Big new feature: simulation classes and console script `tenpy-run` to allow running a simulation.


Changelog
---------

Backwards incompatible changes
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
- Drop official support for Python 3.5
- :meth:`tenpy.linalg.np_conserved.from_ndarray`: raise `ValueError` instead of just a warning in case of the wrong
  non-zero blocks. This behaviour can be switched back with the new argument `raise_wrong_sector`.
- Argument `v0` of :meth:`tenpy.networks.mps.MPS.TransferMatrix.eigenvectors` is renamed to `v0_npc`; `v0` now serves for non-np_conserved guess.


Added
^^^^^
- Simulation class :class:`~tenpy.simulation.simulation.Simulation` and subclasses as a new extra layer for handling the general setup.
- Command line script ``tenpy-run`` and :func:`~tenpy.run_simulation` for setting up a simulation.
- :meth:`~tenpy.networks.mps.MPS.entanglement_entropy_segment2`
- :meth:`tenpy.linalg.sparse.FlatLinearOperator.eigenvectors` and :meth:`~tenpy.linalg.sparse.FlatHermitianOperator.eigenvectors` to unify
  code from :meth:`tenpy.networks.mps.TransferMatrix.eigenvectors` and :meth:`tenpy.linalg.lanczos.lanczos_arpack`.
- :meth:`tenpy.tools.misc.group_by_degeneracy`
- :meth:`tenpy.tools.fit.entropy_profile_from_CFT` and :meth:`tenpy.tools.fit.central_charge_from_S_profile`
- :meth:`tenpy.networks.site.Site.multiply_operators` as a variant of :meth:`~tenpy.networks.site.Site.multiply_op_names` accepting both string and npc arrays.
- :meth:`tenpy.tools.events.EventHandler` to simplify call-backs e.g. for measurement codes during an algorithms.
- :func:`tenpy.tools.misc.find_subclass` to recursively find subclasses of a given base class by the name.
  This function is now used e.g. to find lattice classes given the name, hence supporting user-defined lattices defined outside of TeNPy.
- :func:`tenpy.tools.misc.get_recursive` and :func:`~tenpy.tools.misc.set_recursive` for nested data strucutres, e.g., parameters.
- :func:`tenpy.tools.misc.flatten` to turn a nested data structure into a flat one.
- :class:`tenpy.networks.mps.InitialStateBuilder` to simplify building various initial states.
- Common base class :class:`tenpy.algorithms.Algorithm` for all algorithms.
- :attr:`tenpy.models.lattice.Lattice.Lu` as a class attribute.
- :meth:`tenpy.models.lattice.Lattice.find_coupling_pairs` to automatically find coupling pairs of 'nearest_neighbors' etc..
- :class:`tenpy.models.lattice.HelicalLattice` allowing to have a much smaller MPS unit cell by shifting the boundary conditions around the cylinder.

Changed
^^^^^^^
- For finite DMRG, :cfg:option:`DMRGEngine.N_sweeps_check` now defaults to 1 instead of 10 (which is still the default for infinite MPS).
- Merge :meth:`tenpy.linalg.sparse.FlatLinearOperator.npc_to_flat_all_sectors` into :meth:`~tenpy.linalg.sparse.FlatLinearOperator.npc_to_flat`,
  merge :meth:`tenpy.linalg.sparse.FlatLinearOperator.flat_to_npc_all_sectors` into :meth:`~tenpy.linalg.sparse.FlatLinearOperator.flat_to_npc`.
- Change the ``chinfo.names`` of the specific :class:`~tenpy.networks.site.Site` classes to be more consistent and clear.
- Add the more powerful :meth:`tenpy.networks.site.set_common_charges` to replace :meth:`tenpy.networks.site.multi_sites_combine_charges`.
- Renamed `tenpy.algorithms.tebd.Engine` to :class:`tenpy.algorithms.tebd.TEBDEngine` and
  `tenpy.algorithms.tdvp.Engine` to :class:`tenpy.algorithms.tdvp.TDVPEngine` to have unique algorithm class-names.
- Allow ``swap_op='autoInv'`` for :meth:`tenpy.networks.mps.MPS.swap_sites` and explain the idea of the `swap_op`.

Fixed
^^^^^
- Sign error for the couplings of the :class:`tenpy.models.toric_code.ToricCode`.
- The form of the eigenvectors returned by :meth:`tenpy.networks.mps.TransferMatrix.eigenvectors` 
  was dependent on the `charge_sector` given in the initialization; we try to avoid this now (if possible).
- The charge conserved by ``SpinHalfFermionSite(cons_Sz='parity')`` was wired.
- Allow to pass npc Arrays as Arguments to :meth:`~tenpy.networks.mps.MPS.expectation_value_multi_sites` and
  other correlation functions (:issue:`116`).
- :mod:`tenpy.tools.hdf5_io` did not work with h5py version >= (3,0) due to a change in string encoding (:issue:`117`).
- The overall phase for the returned `W` from :meth:`~tenpy.networks.mps.MPS.compute_K` was undefined.
- :meth:`tenpy.networks.mpo.MPO.expectation_value` didn't work with max_range=0
- The default `trunc_par` for :meth:`tenpy.networks.mps.MPS.swap_sites`, :meth:`~tenpy.networks.mps.MPS.permute_sites` and :meth:`~tenpy.networks.mps.MPS.compute_K` was leading to too small chi for intial MPS with small chi.
