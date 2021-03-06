
project('com.github.stsdc.monitor', 'vala', 'c', version: '0.4.0')

# these are Meson modules
gnome = import('gnome')
i18n = import('i18n')

# and these are project dependencies
glib = dependency('glib-2.0')
granite = dependency('granite')
gtk = dependency('gtk+-3.0')
gee = dependency('gee-0.8')
gio = dependency('gio-2.0')
gobject = dependency('gobject-2.0')
bamf = dependency('libbamf3')
gtop = dependency('libgtop-2.0')
wnck = dependency('libwnck-3.0')
wingpanel = dependency('wingpanel-2.0')

config_data = configuration_data()
config_data.set_quoted('GETTEXT_PACKAGE', meson.project_name())

configure_file(output: 'config.h', configuration: config_data)
config_h_dir = include_directories('.')

icons_gresource = gnome.compile_resources(
    'gresource_icons',
    'data/icons/icons.indicator.gresource.xml',
    source_dir: 'data/icons',
    c_name: 'as'
)

c_args = [
    '-include', 'config.h',
    '-DWNCK_I_KNOW_THIS_IS_UNSTABLE',
    '-w'
]

executable(
    meson.project_name(),
    icons_gresource,
    'src/Monitor.vala',
    'src/MainWindow.vala',
    'src/Utils.vala',

    'src/Widgets/OverallView.vala',
    'src/Widgets/Search.vala',
    'src/Widgets/Headerbar.vala',
    'src/Widgets/Statusbar/Statusbar.vala',

    'src/Models/GenericModel.vala',
    'src/Models/ModelHelper.vala',

    'src/Managers/AppManager.vala',
    'src/Managers/ProcessManager.vala',
    'src/Managers/Process.vala',

    'src/Services/Settings.vala',
    'src/Services/Shortcuts.vala',
    'src/Services/DBusServer.vala',
    'src/Services/Updater.vala',

    'src/Resources/CPU.vala',
    'src/Resources/Core.vala',
    'src/Resources/Memory.vala',
    c_args: c_args,
    dependencies: [
        glib,
        granite,
        gtk,
        gee,
        gio,
        gobject,
        bamf,
        gtop,
        wnck,
        meson.get_compiler('c').find_library('m', required : false),
        meson.get_compiler('vala').find_library('posix')
    ],
    vala_args: [
        '--vapidir=' + meson.source_root() + '/vapi/',
    ],
    install: true
)

shared_module(
    'monitor',
    'src/Indicator/Indicator.vala',
    'src/Utils.vala',

    'src/Indicator/Widgets/DisplayWidget.vala',
    'src/Indicator/Widgets/PopoverWidget.vala',
    'src/Indicator/Widgets/CPUWidget.vala',
    'src/Indicator/Widgets/MemoryWidget.vala',

    'src/Indicator/Services/DBusClient.vala',
    'src/Services/Settings.vala',
    # 'src/Widgets/RevealerSwitch.vala',
    icons_gresource,
    c_args: c_args,
    dependencies: [
        glib,
        granite,
        gobject,
        gtk,
        wingpanel
    ],
    install: true,
    # install_dir : wingpanel.get_pkgconfig_variable('indicatorsdir'), <- installs to /usr/local/
    install_dir : '/usr/lib/x86_64-linux-gnu/wingpanel/'

)

# Add in a post install script
meson.add_install_script('meson/post_install.py')

subdir('data')
subdir('po')

# shows size of binary
# meson.add_postconf_script('du', '-h', meson.project_name())
