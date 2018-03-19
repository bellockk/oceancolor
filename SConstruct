import platform
import SCons

# Add the path for SCons Tools in the Build Tools Submodule
SCons.Tool.DefaultToolpath.insert(0, Dir('tool').abspath)

# Create Build Environment
temp_env = Environment()
if platform.system().lower().startswith('win'):
    temp_env.PrependENVPath(
        "PATH", r'C:\specific\texlive\bin\win32')
elif platform.system().lower().startswith('linux'):
    temp_env.PrependENVPath(
        "PATH", '/specific/texlive/bin/x86_64-linux')
elif platform.system().lower().startswith('darwin'):
    temp_env.PrependENVPath(
        "PATH", '/specific/texlive/bin/universal-darwin')
env = Environment(tools=['default', 'pdflatex'], ENV=temp_env['ENV'])
env.Append(LATEXFLAGS=['-halt-on-error', '-shell-escape'],
           PDFLATEXFLAGS=['-synctex=1', '-halt-on-error', '-shell-escape'])

# Define Options
AddOption('--no-variant-dir',
          dest='variant',
          action='store_false',
          default=True,
          help='do not use a build directory')
env['VARIANT'] = GetOption('variant')

# Export Build Environment
Export('env')

# Build document
if GetOption('variant'):
    kwargs = {'variant_dir':'build', 'duplicate':1}
else:
    kwargs = {}
doc = SConscript('src/SConscript', **kwargs)
if GetOption('variant'):
    Install('dist', doc)
