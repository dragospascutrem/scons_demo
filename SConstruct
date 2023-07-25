EnsureSConsVersion(3, 0)  # for Help() append

import os

# Help message
Help('''
Main SConstruct for Cadence configurations:

This file sets up the build process for Cadence using xrun.
You can use different targets to compile and simulate the design.

Run "scons -n" to see all the commands this file would run.
Run "scons -c" to clean everything.
''', append=True)

# Create a new construction environment
env = Environment(
    ENV={
        'PATH': os.environ['PATH'],
        'HOME': os.environ['HOME'],
    },
    TOOLS=['xrun'],
    # Assuming svscan, fscan, and scanners are defined somewhere in the script
    SCANNERS=[svscan, fscan] + scanners,
    WORK='work',
    SVCONFIG=GetOption('svconfig'),
    XRUN_ALL_ARGS=['-compcnfg', '-exit','-gui'],
)

# Default to -j 8
SetOption('num_jobs', 8)

# Set up the sim_rtl target
sim_rtl = env.AllIn1(
    'simulate_rtl_cadence.log',
    ['libmap_rtl.sv' if env['SVCONFIG'] != '' else 'libmap.sv', 'configs.sv', 'source_code_cadence.f'],
    XRUN_ALL_ARGS=env['XRUN_ALL_ARGS'] + ['-top rtl_config${SVCONFIG}']
)
Alias('sim_rtl', sim_rtl)
Alias('rtl', sim_rtl)

# Set up the sim_cell target
sim_cell = env.AllIn1(
    'simulate_cell_cadence.log',
    ['libmap_rtl.sv', 'configs.sv', 'source_code_cadence.f'],
    XRUN_ALL_ARGS=env['XRUN_ALL_ARGS'] + ['-top cell_config${SVCONFIG}']
)
Alias('sim_cell', sim_cell)
Alias('cell', sim_cell)

# Set up the sim_inst target
sim_inst = env.AllIn1(
    'simulate_inst_cadence.log',
    ['libmap_rtl.sv', 'configs.sv', 'source_code_cadence.f'],
    XRUN_ALL_ARGS=env['XRUN_ALL_ARGS'] + ['-top inst_config${SVCONFIG}']
)
Alias('sim_inst', sim_inst)
Alias('inst', sim_inst)

# Set up additional cleanup
Clean([sim_rtl, sim_cell, sim_inst], [Glob('*~')])
