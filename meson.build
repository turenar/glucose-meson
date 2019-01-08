project('glucose', 'cpp',
	default_options: [
		'warning_level=1',
		'cpp_std=c++11'])

add_project_arguments(
	'-Wno-parentheses',
	'-D__STDC_LIMIT_MACROS',
	'-D__STDC_FORMAT_MACROS',
	language: 'cpp')

compiler = meson.get_compiler('cpp')
lz = compiler.find_library('z')
pthreads = dependency('threads')

inc = include_directories('.')

glucose_lib = library('glucose', [
	'./utils/Options.cc',
	'./utils/System.cc',
	'./core/Solver.cc',
	'./simp/SimpSolver.cc'],
	dependencies: lz)
glocose_dep = declare_dependency(
	include_directories: include_directories('.', is_system: true),
	link_with: glucose_lib,
	dependencies: lz)
syrup_lib = library('glucose-syrup', [
	'./parallel/ClausesBuffer.cc',
	'./parallel/MultiSolvers.cc',
	'./parallel/SolverConfiguration.cc',
	'./parallel/SharedCompanion.cc',
	'./parallel/ParallelSolver.cc',
	'./parallel/SolverCompanion.cc'],
	link_with: glucose_lib,
	dependencies: pthreads)
syrup_dep = declare_dependency(
	include_directories: include_directories('.', is_system: true),
	link_with: syrup_lib,
	dependencies: [syrup_dep, pthreads])
glucose_bin = executable('glucose', './simp/Main.cc',
	link_with: glucose_lib,
	dependencies: lz)
syrup_bin = executable('glucose-syrup', './parallel/Main.cc',
	link_with: [glucose_lib, syrup_lib],
	dependencies: [lz, pthreads])

