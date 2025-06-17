from building import *
import os

# Import environment variables
Import('env')

# Get the current working directory
cwd = GetCurrentDir()

# Initialize include paths and source files list
path = [os.path.join(cwd, 'Include')]
src = [os.path.join(cwd, 'Source', 'Templates', 'system_stm32wbxx.c')]

# Map microcontroller units (MCUs) to their corresponding startup files
mcu_startup_files = {
    'STM32WB55xx': 'startup_stm32wb55xx_cm4.s',
    'STM32WB5Mxx': 'startup_stm32wb5mxx_cm4.s',
    'STM32WB50xx': 'startup_stm32wb50xx_cm4.s',
    'STM32WB35xx': 'startup_stm32wb35xx_cm4.s',
    'STM32WB30xx': 'startup_stm32wb30xx_cm4.s',
    'STM32WB15xx': 'startup_stm32wb15xx_cm4.s',
    'STM32WB10xx': 'startup_stm32wb10xx_cm4.s',

}

# Check each defined MCU, match the platform and append the appropriate startup file
cpp_defines_tuple = env.get('CPPDEFINES', [])
cpp_defines_list = [item[0] if isinstance(item, tuple) else item for item in cpp_defines_tuple]
for mcu, startup_file in mcu_startup_files.items():
    if mcu in cpp_defines_list:
        if rtconfig.PLATFORM in ['gcc', 'llvm-arm']:
            src += [os.path.join(cwd, 'Source', 'Templates', 'gcc', startup_file)]
        elif rtconfig.PLATFORM in ['armcc', 'armclang']:
            src += [os.path.join(cwd, 'Source', 'Templates', 'arm', startup_file)]
        elif rtconfig.PLATFORM in ['iccarm']:
            src += [os.path.join(cwd, 'Source', 'Templates', 'iar', startup_file)]
        break

# Define the build group
group = DefineGroup('STM32WB-CMSIS', src, depend=['PKG_USING_STM32WB_CMSIS_DRIVER'], CPPPATH=path)

# Return the build group
Return('group')
