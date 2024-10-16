project(
  'AVR Default Project',
  'c',
  'cpp',
  default_options: [
    'buildtype=minsize',
    'b_lto=true',
    'cpp_rtti=false',
    'cpp_eh_=none',
    'c_std=c11',
    'cpp_std=c++11',
    'b_staticpic=false',
  ],
)

assert(
  meson.is_cross_build(),
  'AVR Projects can only be built in a cross build enviroment',
)

name = 'project'

avrdude_speed = meson.get_external_property('avrdude_speed')
avrdude_programmer = meson.get_external_property('avrdude_programmer')
requires_reset = meson.get_external_property('requires_reset', false)

source_files = ['src/main.cpp']

bin = executable(name + '.bin', source_files, link_args: '')

# avr-objcopy -O ihex -R .eeprom $< $@
objcopy = find_program('avr-objcopy')
hex = custom_target(
  'hex',
  input: bin,
  output: name + '.hex',
  command: [objcopy, '-O', 'ihex', '-R', '.eeprom', '@INPUT@', '@OUTPUT@'],
)

# avrdude -F -V -c arduino -p ATMEGA328P -P /dev/ttyACM0 -b 115200 -U flash:w:Build/test.hex
if requires_reset
  avrdude = find_program('avrdude-wrapper.sh')
else
  avrdude = find_program('avrdude')
endif
run_target(
  'push',
  command: [
    avrdude,
    '-F',
    '-v',
    '-p' + target_machine.cpu(),
    '-P' + get_option('port'),
    '-c' + avrdude_programmer,
    '-b' + avrdude_speed,
    '-D',
    '-Uflash:w:@0@:i'.format(hex.full_path()),
  ],
  depends: hex,
)