project('doocs-json', 'cpp',
  version : '0.1.0',
  default_options : [
    'warning_level=3',
    'cpp_std=c++17'
  ]
)

doocsapi_dep = dependency('doocs-doocsapi', fallback : [ 'doocs-clientlib', 'doocsapi_dep' ])

# use nlohmann-json3-dev on Ubuntu
nlohmann_json_dep = dependency('nlohmann_json', fallback : ['nlohmann_json', 'nlohmann_json_dep'])

gtest_dep = dependency('gtest', main : false)

# rsocket dependency
cmake = import('cmake')

#rsocket_sub_project = cmake.subproject('rsocket')
#rsocket_dep = rsocket_sub_project.dependency('ReactiveSocket')

zmq_dep = meson.get_compiler('cpp').find_library('libzmq')
zmqpp_dep = meson.get_compiler('cpp').find_library('libzmqpp')

include_dir = include_directories('include')

doocs_json_lib = static_library(
  'doocs_json', ['src/doocs_json.cpp'],
  include_directories : include_dir,
  dependencies : [
    doocsapi_dep, 
    nlohmann_json_dep,
  ]
)

doocs_json_dep = declare_dependency(include_directories : 'include',  link_with : doocs_json_lib)

doocs_zmq_lib = static_library(
  'doocs_zmq', ['src/doocs_zmq.cpp'],
  include_directories : include_dir,
  dependencies : [
    doocsapi_dep,
    nlohmann_json_dep,
    zmqpp_dep,
    zmq_dep,
  ]
)

doocs_zmq_dep = declare_dependency(include_directories : 'include',  link_with : [doocs_json_lib, doocs_zmq_lib])

zmq_exe = executable(
  'doocs_zmq_app',
  'src/doocs_zmq_app.cpp',
  include_directories : include_dir,
  install : true,
  dependencies : doocs_zmq_dep
)

json_exe = executable(
  'doocs_json_app',
  'src/doocs_json_app.cpp',
  include_directories : include_dir,
  install : true,
  dependencies : doocs_json_dep
)

json_test = executable(
  'doocs_json_test',
  [
   'test/json_test_local.cpp',
   'test/json_test_remote.cpp',
  ],
  include_directories : include_dir,
  dependencies : [
    doocs_json_dep,
    gtest_dep,
  ]
)
