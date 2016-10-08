# mriscv

Building a pure RV32I Toolchain
-------------------------------

The default settings in the [riscv-tools](https://github.com/riscv/riscv-tools) build
scripts will build a compiler, assembler and linker that can target any RISC-V ISA,
but the libraries are built for RV32G and RV64G targets. Follow the instructions
below to build a complete toolchain (including libraries) that target a pure RV32I
CPU.

The following commands will build the RISC-V gnu toolchain and libraries for a
pure RV32I target, and install it in `/opt/riscv32i`:

    # Ubuntu packages needed:
    sudo apt-get install autoconf automake autotools-dev curl libmpc-dev libmpfr-dev \
            libgmp-dev gawk build-essential bison flex texinfo gperf libtool patchutils bc

    sudo mkdir /opt/riscv32i
    sudo chown $USER /opt/riscv32i

    git clone https://github.com/riscv/riscv-gnu-toolchain riscv-gnu-toolchain-rv32i
    cd riscv-gnu-toolchain-rv32i
    git checkout 7e48594
    git submodule update --init --recursive

    mkdir build; cd build
    ../configure --with-arch=RV32I --prefix=/opt/riscv32i
    make -j$(nproc)
    
    
The microcontroller
--------------------

The microcontroller is composed by a 32bit RISC-V core, a 4KB SRAM, a 10b ADC, a 12b DAC, 
8 GPIO, and a master and a slave SPI interfaces. All modules are connected using two different
buses: AXI4 and APB.

The directory tree is:

mriscv_axi/ 						
mriscv_axi/ADC_interface_AXI		-----> ADC interface for the AXI4 bus
mriscv_axi/AXI_SP32B1024			-----> the AXI4 bus
mriscv_axi/DAC_interface_AXI		-----> DAC intreface for the AXI4 bus
mriscv_axi/axi4_interconnect		-----> Memory interface for AXI4
mriscv_axi/impl_axi					-----> The microncontroller
mriscv_axi/spi_axi_master			-----> master SPI for programming
mriscv_axi/spi_axi_slave			-----> slave SPI for 

