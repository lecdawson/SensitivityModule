# SensitivityModule readme


Cheryl Patrick (UCL)
Nov 17, 2017

SensitivityModule  is a Falaise pipeline module to process selected data from the SD, CD, TCD, TTD and PTD banks and output a ROOT ntuple file. It has significant overlap with the ParticleID module, but is not the same. In the long term, there is scope for consolidation.


## Files:

SensitivityModule.cpp

SensitivityModule_template.conf

SensitivityModule.h

CMakeLists_template.txt


## Description

Add to an flreconstruct pipeline to generate a ROOT ntuple file with some pertinent branches. SensitivityModule.conf gives an example of how I run it; in this case my input file would be something that had already been through the full flreconstruct pipeline (up to and including gamma clustering), and that includes SD, CD, TCD, TTD, PTD banks. The output file will always be called sensitivity.root so don’t run it multiple times concurrently in the same directory!

CMakeLists.txt will build it on my Mac; I don’t guarantee (or even expect) it will work everywhere. In fact I know it doesn’t, but you can use it as a starting point. You'll have to at the very least change the paths.

CMakeLists_template.txt and SensitivityModule_template.conf are template files with hardcoded paths on Cheryl's computer. Copy them, remove underscore and template from the names and edit them to point at your own paths.

Use the falaise flreconstruct pipeline instructions to see how to build and integrate this module to your pipeline.

## Output tuple structure


**sensitivity.total_calorimeter_energy** : Summed energy of all reconstructed calorimeter hits (CD bank)

**sensitivity.passes_two_calorimeters** : True if there are exactly 2 reconstructed calorimeter hits over 50 keV, of which at least 1 is over 150keV (CD bank)

**sensitivity.passes_two_plus_calos**  :True if there are at least 2 reconstructed calorimeter hits over 50 keV, of which at least 1 is over 150keV (CD bank)

**sensitivity.passes_two_clusters**  : True if there are exactly two reconstructed clusters with 3 or more hits (TCD bank)

**sensitivity.passes_two_tracks** : True if there are exactly two reconstructed tracks (TTD bank)

**sensitivity.passes_associated_calorimeters** : True if there are exactly two tracks, and they are both associated to one or more calorimeter hits. Equivalent (at the moment to a 2-electron topology)

**sensitivity.number_of_electrons** : Number of electron-candidate tracks - that is, tracks that are associated to one or more calorimeter hits. No check on the charge as of yet (but you can do it yourself looking at electron_charges). No requirement for a foil vertex. (PTD bank)

**sensitivity.number_of_gammas** : Number of gamma candidates - when calorimeter hits that aren’t associated to tracks have been grouped by the gamma tracko-clustering algorithm to correspond to what appear to be individual gammas (PTD bank)

**sensitivity.higher_electron_energy** : Energy of the highest-energy electron candidate, summed over all associated calorimeter hits (at the moment I don’t think more than 1 hit is allowed, but that could change in future). 0 if no electron candidates. Corresponds to ** sensitivity.electron_energies[0].

**sensitivity.lower_electron_energy** : Energy of the second-highest-energy electron candidate, summed over all associated calorimeter hits (at the moment I don’t think more than 1 hit is allowed, but that could change in future). 0 if less than 2 electron candidates. Corresponds to sensitivity.electron_energies[1].

**sensitivity.electron_energies** : Vector of all electron-candidate energies. In descending order of energy.

**sensitivity.electron_charges** : Vector of all electron-candidate charges. In descending order of energy. 1 = undefined (straight track), 4 = positive, 8 = negative.

**sensitivity.gamma_energies** : Vector of all electron-candidate energies. In descending order of energy.

**sensitivity.true_higher_electron_energy** :  Same as true_highest_primary_energy

**sensitivity.true_lower_electron_energy** : Same as  sensitivity.true_second_primary_energy

**sensitivity.true_highest_primary_energy** : The energy of the highest-energy primary particle in the interaction (from SD bank)

**sensitivity.true_second_primary_energy** : The energy of the second-highest-energy primary particle in the interaction (from SD bank)

**sensitivity.true_total_energy** : Summed energy of every primary particle in the interaction (from SD bank)

**sensitivity.true_vertex_x** : X coordinate of the true event vertex (generator level, from SD bank). Foil is at x ~ 0, main calo walls are at +/- 434.994 mm according to flvisualize. Unit is mm.

**sensitivity.true_vertex_y** : Y coordinate of the true event vertex (generator level, from SD bank).  The y direction is horizontal, parallel to the foil, you can see it in top view. Unit is mm.

**sensitivity.true_vertex_z** : Z coordinate of the true event vertex (generator level, from SD bank).  The z direction is vertical, parallel to the wires, you can see it in side view. Unit is mm.

**sensitivity.true_higher_particle_type** : The particle type (electon, gamma, positron) of the highest energy particle
in the event. (1 - gamma, 3-electron, 47- alpha)

**sensitivity.true_lower_particle_type** : The particle type (electon, gamma, positron) of the lowest energy particle
in the event. (1 - gamma, 3-electron, 47- alpha)

**sensitivity.first_vertex_x** : If there are two tracks, vertex x position of an arbitrary “first” track. Foil is at x ~ 0, main calo walls are at +/- 434.994 mm according to flvisualize. Unit is mm.

**sensitivity.first_vertex_y** : If there are two tracks, vertex y position of an arbitrary “first” track. The y direction is horizontal, parallel to the foil, you can see it in top view. Unit is mm.

**sensitivity.first_vertex_z** : If there are two tracks, vertex z position of an arbitrary “first” track. The z direction is vertical, parallel to the wires, you can see it in side view. Unit is mm.

**sensitivity.second_vertex_x** : If there are two tracks, vertex x position of an arbitrary “2nd” track. Foil is at x ~ 0, main calo walls are at +/- 434.994 mm according to flvisualize. Unit is mm.

**sensitivity.second_vertex_y** :  If there are two tracks, vertex y position of an arbitrary “2nd” track. The y direction is horizontal, parallel to the foil, you can see it in top view. Unit is mm.

**sensitivity.second_vertex_z** : If there are two tracks, vertex z position of an arbitrary “2nd” track. The z direction is vertical, parallel to the wires, you can see it in side view. Unit is mm.

**sensitivity.vertex_separation** : Distance between the inner-most (nearest to the foil) vertices of the two tracks (only if there are 2 electron candidates).  Unit is mm.

**sensitivity.first_projected_vertex_y; sensitivity.first_projected_vertex_z; sensitivity.second_projected_vertex_y; sensitivity.second_projected_vertex_z** : Only for 2-electron or 1e-n-gamma topologies. If both tracks were linearly projected back to the foil (x=0), these would be their y and z coordinates.  Unit is mm.

**sensitivity.foil_projection_separation** : Only if there are 2 electron candidates. If both tracks were linearly projected back to the foil, this is the distance between where the tracks intersect the foil.  Unit is mm.

**sensitivity.projection_distance_xy** : How far in the xy plane we had to project our longest-projected track. Proxy for how many cells we are claiming to have a track, but where we didn’t reconstruct a hit. Could be replaced by a mapping of broken cells etc.

**sensitivity.vertices_on_foil** : If 2 tracks: number of tracks with a vertex on the foil.

**sensitivity.first_vertices_on_foil** : Obsolete

**sensitivity.angle_between_tracks** : For 2-electron events: Angle between the initial momentum vectors of the two tracks. Does not require them to share a vertex (maybe it should). For 1-electron-n-gamma events, angle between the electron track and the highest-energy gamma “track”, if we assume that the gamma travels from the foil-most electron vertex to the centre of the calorimeter that it hits first.

**sensitivity.same_side_of_foil** : If 2 tracks: True if both tracks are on the same side of the foil, false if not

**sensitivity.first_track_direction_x; sensitivity.first_track_direction_y ;                          **sensitivity.first_track_direction_z; sensitivity.second_track_direction_x;                              **sensitivity.second_track_direction_y ; sensitivity.second_track_direction_z** :  Initial direction vectors for the two tracks (Only if two tracks, arbitrary which is which)

**sensitivity.time_delay** : If 2 calorimeter hits  - time delay in nanoseconds between the hits. Used in the past as a crude proxy for internal/external probability.

**sensitivity.traj_cluster_delayed_time** : If a delayed track, the time of the first delayed hit in ns

**sensitivity.topology_2e** : True if event has a 2-electron topology (2 tracks with associated calorimeter hits, no gammas, no other tracks). False if not.

**sensitivity.internal_probability** :  Calculates the probability that it is an internal event (both tracks are initiated in the foil). Available for 2-electron events, or 1-electron-n-gamma events, in which case it uses the path of the highest energy gamma. If internal, this should be equally distributed from 0 to 1. If external (particle leaves one calorimeter, travels to foil, then to another calorimeter) this will be very close to 0. Calculated from energy and time of calorimeter hits vs length of tracks.

**sensitivity.internal_chi_squared** :  Intermediate step to calculating internal_probability

**sensitivity.external_probability** : If 2 associated tracks, this calculates the probability that it is an external event (particle leaves one calorimeter, travels to foil, then to another calorimeter).   Available for 2-electron events, or 1-electron-n-gamma events, in which case it uses the path of the highest energy gamma. If external, this should be equally distributed from 0 to 1. If internal  (both tracks are initiated in the foil) this will be very close to 0. Calculated from energy and time of calorimeter hits vs length of tracks.

**sensitivity.external_chi_squared** :  Intermediate step to calculating external_probability

**sensitivity.foil_projected_internal_probability;
sensitivity.foil_projected_external_probability** : As internal and external probability, if each track’s length were extended to project the track linearly back to the foil

**sensitivity.topology_1e1gamma** : True if event has 1 electron candidate (track with associated hits) and 1 gamma candidate (collection of unassociated hits with timings corresponding to a single gamma)

**sensitivity.topology_1engamma** : True if event has 1 electron candidate (track with associated hits) and 1 or more gamma candidates (collections of unassociated hits with timings corresponding to a gamma)

**sensitivity.calorimeter_hit_count** : Number of calorimeter hits

**sensitivity.cluster_count** : Number of clusters with 3 or more hits

**sensitivity.track_count** : Number of tracks in the tracker

**sensitivity.associated_track_count** : Number of electron candidates

**sensitivity.alpha_count** : Not used yet

**sensitivity.foil_alpha_count** : Not used yet

**sensitivity.delayed_cluster_hit_count** : Number of hits in the delayed cluster. Used to determine the correct
metric for calculating the alpha track and alpha projected track lengths

**sensitivity.latest_delayed_hit** : Not used yet

**sensitivity.small_cluster_count** : Number of clusters with 2 hits

**sensitivity.third_calo_energy** : copy of highest_gamma_energy, name kept for legacy

**sensitivity.highest_gamma_energy** : Highest energy gamma, may come from more than 1 calorimeter hit as specified by gamma tracko- clustering

**sensitivity.edgemost_vertex** : Absolute y position (in mm) of the vertex that is nearest to the edge of the detector in the y dimension. This could possibly be used with small cluster identification to find events near the edge of the detector who have two tracks, each associated with a calorimeter and with close vertices on the foil, but for one of which there are only 2 hits (because it is too near the edge to pass through 3 cells).

**sensitivity.alpha_track_length** : Length in mm of the delayed track. This length is calculated in different
ways depending on the number of hits in the delayed cluster. This is due to the way < 3 hit tracks are treated by
alpha finder. More detail is provided in code comments.

**sensitivity.proj_track_length_alpha** : Length in mm of delayed track when it is projected back to the back to the
electron projeted foil vertex. Again this calculation is different for delayed tracks with 1,2 or >2 hits. See code for
more detail.

**sensitivity.gamma_fractions_mainwall;
sensitivity.gamma_fractions_xwall;
sensitivity.gamma_fractions_gveto** :
Vectors corresponding to each reconstructed gamma in the event, starting from the most energetic and decreasing in energy order. For each one, they give the fraction of the energy of that gamma that was deposited in the given wall. (With gamma tracker, a given gamma can scatter to several calorimeters). (Therefore, for each entry number, the values for the 3 vectors should add up to 1).

**sensitivity.gamma_hits_mainwall;
sensitivity.gamma_hits_xwall;
sensitivity.gamma_hits_gveto** :
Vectors corresponding to each reconstructed gamma in the event, starting from the most energetic and decreasing in energy order. For each one, if the FIRST (in time) hit for that gamma was in the specified wall, it will be true, otherwise false. (Therefore, for each entry number, only 1 of these three should be true).

**sensitivity.electron_hits_mainwall;
sensitivity.electron_hits_xwall;
sensitivity.electron_hits_gveto** :
Vectors corresponding to each reconstructed electron in the event, starting from the most energetic and decreasing in energy order. For each one, if the associated calo hit for that electron was in the specified wall, it will be true, otherwise false. (Therefore, for each entry number, only 1 of these three should be true).
