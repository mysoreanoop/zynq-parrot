include ../Makefile.design

#############################
# Modify base flist
#############################
BASE_FLIST ?= $(abspath ../flist.vcs)
FLIST      ?= $(abspath flist.vcs)
$(FLIST): $(BLACKPARROT_DIR)/bp_top/syn/flist.vcs $(BASE_FLIST)
	cat $^ | envsubst > $@
	echo "+incdir+$(CURR_SRC_DIR)" >> $@
	echo "+incdir+$(COSIM_DIR)/include/vivado" >> $@
	# Replace hardened memory which is incorrectly inferred on some Xilinx FPGAs
	sed -i "/bsg_mem_1rw_sync_mask_write_bit.v/d" $@
	sed -i "/bsg_mem_1rw_sync_mask_write_bit_synth.v/d" $@
	echo "$(BASEJUMP_STL_DIR)/hard/ultrascale_plus/bsg_mem/bsg_mem_1rw_sync_mask_write_bit.v" >> $@
	# Replace bsg_mux with one that uses Xilinx macros for correct mapping of logic on clock-tree
	sed -i "s|.*/bsg_mux.v|$(BASEJUMP_STL_DIR)/hard/ultrascale_plus/bsg_misc/bsg_mux.v|" $@
	sed -i "s|.*/bsg_clkgate_optional.v|$(BASEJUMP_STL_DIR)/hard/ultrascale_plus/bsg_misc/bsg_clkgate_optional.v|" $@

include $(TOP)/cosim/mk/Makefile.vivado

