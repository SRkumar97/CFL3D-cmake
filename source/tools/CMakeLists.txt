cmake_minimum_required(VERSION 2.6)
enable_language(Fortran)

set(
TOOLS_FSRCS
grid_perturb_cmplx.F 
grid_perturb.F
Get_FD.F
moovmaker.F
plot3dg_to_cgns.F
XINTOUT_to_ovrlp.F
p3d_to_INGRID.F
INGRID_to_p3d.F
cfl3d_to_pegbc.F
everyother_xyz.F
initialize_field.F
p3d_to_cfl3drst.F
v6_restart_mod.F
v6inpdoubhalf.F
cgns_to_cfl3dinput.F
cgns_readhist.F
v6_ronnie_mod.F90
cfl3d_to_nmf.F
cfl3dinp_to_FVBND.F
nmf_to_cfl3dinput.F
gridswitchijk.F
v6inpswitchijk.F
)

set(TOOLS_SHARED_FSRCS
../cfl3d/dist/umalloc_c.F ../cfl3d/dist/umalloc_r.F ../cfl3d/dist/termn8.F 
 ../cfl3d/libs/readkey.F ../cfl3d/libs/global0.F ../cfl3d/libs/cgnstools.F ../cfl3d/libs/parser.F ../cfl3d/libs/outbuf.F ../cfl3d/libs/my_flush.F
)

add_library(TOOLS_SHARED STATIC ${TOOLS_SHARED_FSRCS})
if (USE_CGNS)
	target_link_libraries(TOOLS_SHARED PUBLIC cgns)
endif (USE_CGNS)

# add dummy target to group the tools
add_custom_target(tools ALL)
foreach (tool_src ${TOOLS_FSRCS})
	get_filename_component(tool ${tool_src} NAME_WE)
	add_executable(${tool} ${tool_src})
	target_link_libraries(${tool} PUBLIC TOOLS_SHARED)
	target_link_libraries(${tool} PUBLIC dist)
	target_link_libraries(${tool} PUBLIC common)
	add_dependencies(tools ${tool})
endforeach (tool_src)
