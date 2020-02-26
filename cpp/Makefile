
##########################################################
## EDIT THESE PARAMETERS
##########################################################
CC := mpiicpc
CFLAGS := -O3
PNETCDF_PATH=/lustre/home/z04/lcebaman/libs/PnetCDF
INCLUDE := -I${PNETCDF_PATH}/include
LDFLAGS := -L${PNETCDF_PATH}/lib -lpnetcdf -L${LAPACK_PATH}/lib
OMPFLAGS := -mp
ACCFLAGS := -ta=tesla,pinned,cc50,cuda9.0,ptxinfo -Minfo=accel
##########################################################
## END EDITING SECTION
##########################################################
ifeq ($(DEBUG),1)
OPTS = -g
endif

ifndef NX  #at this point, the makefile checks if NX is enabled
OPTS += -D_NX=100  #variable passed to the compiler
else
OPTS += -D_NX=$(NX)
endif

ifndef NZ  #at this point, the makefile checks if NZ is enabled
OPTS += -D_NZ=50 #variable passed to the compiler
else
OPTS += -D_NZ=$(NZ) #variable passed to the compiler
endif

ifndef SIMTIME  #at this point, the makefile checks if NZ is enabled
OPTS += -D_SIM_TIME=1000 #variable passed to the compiler
else
OPTS += -D_SIM_TIME=$(SIMTIME) #variable passed to the compiler
endif

ifndef IOFREQ  #at this point, the makefile checks if NZ is enabled
OPTS += -D_OUT_FREQ=10 #variable passed to the compiler
else
OPTS += -D_OUT_FREQ=$(IOFREQ) #variable passed to the compiler
endif

ifndef DATASPEC  #at this point, the makefile checks if NZ is enabled
OPTS += -D_DATA_SPEC=2 #variable passed to the compiler
else
OPTS += -D_DATA_SPEC=$(DATASPEC) #variable passed to the compiler
endif

all: serial mpi openmp openacc

serial:
	${CC} ${INCLUDE} ${CFLAGS} -o miniWeather_serial miniWeather_serial.cpp ${LDFLAGS}

mpi:
	${CC} ${INCLUDE} ${CFLAGS} ${OPTS} -o miniWeather_mpi miniWeather_mpi.cpp ${LDFLAGS}

openmp:
	${CC} ${INCLUDE} ${CFLAGS} ${OMPFLAGS} -o miniWeather_mpi_openmp miniWeather_mpi_openmp.cpp ${LDFLAGS}

openacc:
	${CC} ${INCLUDE} ${CFLAGS} ${ACCFLAGS} -o miniWeather_mpi_openacc miniWeather_mpi_openacc.cpp ${LDFLAGS}

clean:
	rm -f *.o *.mod miniWeather_serial miniWeather_mpi miniWeather_mpi_openmp miniWeather_mpi_openacc
