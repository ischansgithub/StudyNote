从github上下载opendr-master源码。

将OSMesa.Linux.x86_64.zip文件（百度网盘中）放入opendr-master/opendr/contexts/下面

## Some Keypoints

在虚拟环境下执行

```shell
$python setup.py build
```

会报错:

```shell
(hmr_env) chenww@ubuntu:~/hmr/dependencies/opendr-master$ python setup.py build
running build
running build_py
running build_ext
building 'opendr.contexts.ctx_mesa' extension
gcc -pthread -B /home/chenww/yes/envs/hmr_env/compiler_compat -Wl,--sysroot=/ -fno-strict-aliasing -g -O2 -DNDEBUG -g -fwrapv -O3 -Wall -Wstrict-prototypes -fPIC -D__OSMESA__=1 -Iopendr/contexts -I. -I/home/chenww/yes/envs/hmr_env/lib/python2.7/site-packages/numpy/core/include -Iopendr/contexts/OSMesa/include -I/home/chenww/yes/envs/hmr_env/include/python2.7 -c opendr/contexts/ctx_mesa.c -o build/temp.linux-x86_64-2.7/opendr/contexts/ctx_mesa.o -lstdc++
In file included from /home/chenww/yes/envs/hmr_env/lib/python2.7/site-packages/numpy/core/include/numpy/ndarraytypes.h:1824:0,
                 from /home/chenww/yes/envs/hmr_env/lib/python2.7/site-packages/numpy/core/include/numpy/ndarrayobject.h:12,
                 from /home/chenww/yes/envs/hmr_env/lib/python2.7/site-packages/numpy/core/include/numpy/arrayobject.h:4,
                 from opendr/contexts/ctx_mesa.c:613:
/home/chenww/yes/envs/hmr_env/lib/python2.7/site-packages/numpy/core/include/numpy/npy_1_7_deprecated_api.h:17:2: warning: #warning "Using deprecated NumPy API, disable it with " "#define NPY_NO_DEPRECATED_API NPY_1_7_API_VERSION" [-Wcpp]
 #warning "Using deprecated NumPy API, disable it with " \
  ^~~~~~~
In file included from opendr/contexts/ctx_mesa.c:616:0:
opendr/contexts/OSMesa/include/GL/osmesa.h:258:1: warning: function declaration isn’t a prototype [-Wstrict-prototypes]
 typedef void (*OSMESAproc)();
 ^~~~~~~
opendr/contexts/ctx_mesa.c: In function ‘__pyx_pf_6opendr_8contexts_8ctx_mesa_13OsContextBase_150ShaderSource’:
opendr/contexts/ctx_mesa.c:13355:49: warning: passing argument 3 of ‘glShaderSource’ from incompatible pointer type [-Wincompatible-pointer-types]
   glShaderSource(__pyx_v_shader, __pyx_v_count, (&__pyx_v_s), (&__pyx_v_len));
                                                 ^
In file included from opendr/contexts/OSMesa/include/GL/gl.h:2085:0,
                 from opendr/contexts/gl_includes.h:10,
                 from opendr/contexts/ctx_mesa.c:615:
opendr/contexts/OSMesa/include/GL/glext.h:5794:21: note: expected ‘const GLchar ** {aka const char **}’ but argument is of type ‘char **’
 GLAPI void APIENTRY glShaderSource (GLuint shader, GLsizei count, const GLchar* *string, const GLint *length);
                     ^~~~~~~~~~~~~~
gcc -pthread -shared -B /home/chenww/yes/envs/hmr_env/compiler_compat -L/home/chenww/yes/envs/hmr_env/lib -Wl,-rpath=/home/chenww/yes/envs/hmr_env/lib -Wl,--no-as-needed -Wl,--sysroot=/ build/temp.linux-x86_64-2.7/opendr/contexts/ctx_mesa.o -Lopendr/contexts/OSMesa/lib -L/home/chenww/yes/envs/hmr_env/lib -lOSMesa -lGL -lGLU -lpython2.7 -o build/lib.linux-x86_64-2.7/opendr/contexts/ctx_mesa.so -lstdc++
/home/chenww/yes/envs/hmr_env/compiler_compat/ld: cannot find -lstdc++
collect2: error: ld returned 1 exit status
error: command 'gcc' failed with exit status 1
```

但是我conda deactivate 退出虚拟环境后就build成功了。这个有待试验。

## 问题1

python setup.py build报错：找不到ctx_mesa.c
但实际上在opendr-master/opendr/contexts/下有ctx_mesa.pyx.

查看setup.py并实验发现

try:
    from Cython.Build import cythonize
    have_cython = True
    print(have_cython)
except:
    cythonize = lambda x : x
    have_cython = False
    print(have_cython)
中from Cython.Build import cythonize会报错导致have_cython ＝ false，进一步导致setup.py一定要找到ctx_mesa.c。

解决：下载最新版本的Cython-0.28.1即可。如果from Cython.Build import cythonize报错可能的原因是Cython不是最新版本的。

## 问题2

- 在公共环境中已经安装好opendr，但在虚拟环境中无法安装Opendr。

- 解决：哪里报错找不到Opendr,就在哪里添加Python包的路径

  ```python
  import sys
  sys.path.append('/usr/local/lib/python2.7/dist-packages/opendr-0.77-py2.7-linux-x86_64.egg')
  ```

  





