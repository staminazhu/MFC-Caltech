include ../../Makefile.in

# Find all source files, create a list of corresponding object files
SRCS=$(wildcard *.f90)
OBJS=$(patsubst %.f90,%.o,$(SRCS))

# Ditto for mods (They will be in both lists)
MODS=$(wildcard m*.f90)
MOD_OBJS=$(patsubst %.f90,%.o,$(MODS))

# Compiler/Linker settings
FLFLAGS = -g $(ld_post_flags)
FCFLAGS = -g $(f90_post_flags)
EXEC = post_process
PROGRAM = p_main
PRG_OBJ = $(PROGRAM).o

# make without parameters will make first target found.
all : $(PROGRAM)

# Compiler steps for all objects
$(OBJS) : %.o : %.f90
	$(FC) $(FCFLAGS) -o $@ $<

# Linker
$(PROGRAM) : $(OBJS)
	$(FC) -o $(EXEC) $(OBJS) $(FLFLAGS)

.PHONY: all clean

clean:
	rm -rf $(OBJS) $(PROGRAM) $(EXEC)  $(patsubst %.o,%.mod,$(MOD_OBJS))

# Dependencies

# Main program depends on all modules
$(PRG_OBJ) : $(MOD_OBJS)

# Modules depend on each other
m_data_input.o :            m_derived_types.o m_global_parameters.o m_mpi_proxy.o m_compile_specific.o
m_data_output.o :           m_derived_types.o m_global_parameters.o m_mpi_proxy.o m_compile_specific.o
m_start_up.o :              m_derived_types.o m_global_parameters.o m_mpi_proxy.o m_compile_specific.o
m_fftw.o :                  m_derived_types.o m_global_parameters.o m_mpi_proxy.o
m_derived_variables.o :     m_derived_types.o m_global_parameters.o m_mpi_proxy.o
m_mpi_proxy.o :             m_derived_types.o m_global_parameters.o
m_variables_conversion.o :  m_derived_types.o m_global_parameters.o
m_global_parameters.o :     m_derived_types.o
