---
layout: post
published: true
title: WSGI �Ķ��ʼ�
description: just simple reading note
---  

#WSGI��Ŀ��
WSGI ��ȫ���� Web Server Gateway Interface

WSGI��web server��Python webӦ�ã�web applications�����߿��֮��Ľӿڡ���Ŀ���Ǵٽ�webӦ�õĿ���ֲ�ԡ���Ϊweb server�кܶ��֣�webӦ��Ҳ�кܶ��֣����û��һ���淶����ô������дwebӦ�õ�ʱ��ֻ�����ĳһ��server��д������û�п���ֲ�ԡ����������WSGI�淶����ô���ǵ�webӦ�ÿ���ʹ���κ�����WSGI��server��

������һ�����⣺��WSGI�����ǰ���ܶ��server��Ӧ�ö��Ѿ��������п��ˣ���Щserver��application�϶�û������WSGI������WSGI����Щ�Ѿ���ʹ�õ�server��applicationû��̫��ô���Ϊ�ˣ�WSGI�����ر�򵥶�������ʵ�֣����Ѿ����ڵ�server��Ӧ��ֻ��Ҫ���ٵĸ��ļ���������Щ�淶���������˲��ж�������WSGI��

#�淶����
WSGI�ӿ������ˣ�1)server�˻���˵��gateway���� 2)Ӧ�ö˻��ߡ���ܡ��ˡ�server�˵�����Ӧ�ö��ṩ�Ŀɵ��ö���callable object�������ڵ��õķ�ʽȡ����server�ˡ������callableָ���� **һ���������������࣬����ӵ��call�����Ķ���ʵ��**��server��Ӧ���ڵ���callable�����ʱ�򣬲������������callable��������ͣ������Ǻ��������ࣩ��callable��������ܹ������ã�������ʡ��introspect��

���˴����server/gateways��applications/frameworks�������м����middleware)����������֮�䡣����server���ԣ��м�����ֵ�����һ��Ӧ�ã�������Ӧ�ö��ԣ��м�����ֵ�����һ��server���м������ǿ�󣬿����ṩ����չ��API������ת���͵����ȵȹ��ܡ�

#Ӧ��/��ܶ�
Ӧ�õĶ�����һ���������������Ŀɵ��ö�������Ŀɵ��ö�����������ᵽ�ġ�callable object����һ���������������࣬���ߴ���*__call__* �����Ķ��󶼿�����������Ӧ�ó��������Ϊserver�˶��ᷢ�ظ�����������Ӧ�õĶ���Ҳ�����ܱ���ε��ã����صĽ��ҲҪ��**�ɵ�����**��Ӧ�ó�����������ﲢ����ָӦ�ó��򿪷��ߵ���WSGI��API������ **���ڼٶ�Ӧ�ó��򿪷�����Ȼʹ�����еġ��߲�εĿ�ܣ���flask��������Ӧ�ó���WSGI��ʵ�ǿ�ܺ�server�˵Ŀ����淶������ֱ��֧��Ӧ�ÿ�����**


������Ӧ�ó��������������ӣ�һ���Ǻ�����һ�����ࣺ
```python
def simple_app(environ, start_response):
    """ Ҳ���������򵥵�application object """
    status = '200 OK'
    response_headers = [('Content-type', 'text/plain')]
    start_response(status, response_headers)
    return ['Hello world!\n']


class AppClass:
    """���ͬ���Ľ��������ʹ����

    ע��: 'AppClass'�� ��������һ�� "application" ,���Ե������᷵��һ��AppClass��ʵ���� 
    ���ʵ��������ķ��ؽ����
    ���������Ҫʹ��AppClass��ʵ����Ϊ"application"����ô���Ǳ���Ҫʵ��__call__������
    """

    def __init__(self, environ, start_response):
        self.environ = environ
        self.start = start_response

    def __iter__(self):
        status = '200 OK'
        response_headers = [('Content-type', 'text/plain')]
        self.start(status, response_headers)
        yield "Hello world!\n"
```

Ӧ�ó����������������λ�ò�����positional arguments�����ֱ�Ϊ*environ* ��*start_response*�����������ֲ�һ��Ҫ�����������ʣ�����λ�ú����岻�ܱ䡣*environ*������һ���ֵ����Ҳ��һ������CGI���Ļ���������Ӧ�ó�������������κ�����Ҫ�ķ�ʽ���޸�����ֵ䣬 *environ*���������һЩ�ض���WSGI����ı�����*start_response*������һ���ɱ����õĺ�����������������Ҫ��λ�ò�����һ����ѡ������status��response_headers�� exc_info ��ͬ���ģ����ֿ�������ȡ������λ�úʹ���ĺ��岻�ܱ䡣status������һ����ʽ�硰999 Message here��������״̬�ַ�������response_headers������һ�������У�header_name,header_value�������б��Ԫ�飬��������HTTP����Ӧͷ����ѡ����exc_info��Ҫ�Ǵ������ʱʹ�á�

start_response���뷵��һ��write(*body_data*)�ɱ����õĶ��󣬸������write����һ������ߺ�����write�Ὣ����д��http����Ӧbody���档���ǹ淶��Ҳ�ᵽ���µĿ��Ӧ�ñ���ʹ��write��

----------

#server��/���ض�
server��ÿ�ν���HTTP�ͻ��˵�����͵���applicationһ�Ρ�������һ���򵥵�CGI���أ���һ����������ʽ��ʵ�֡�����������漰���Ĵ�������٣���ΪĬ��û�б���׽�����쳣����dump��sys.stderr���У�����web server ����log�С�

```python
import os, sys

def run_with_cgi(application):

    environ = dict(os.environ.items())
    environ['wsgi.input']        = sys.stdin
    environ['wsgi.errors']       = sys.stderr
    environ['wsgi.version']      = (1, 0)
    environ['wsgi.multithread']  = False
    environ['wsgi.multiprocess'] = True
    environ['wsgi.run_once']     = True

    if environ.get('HTTPS', 'off') in ('on', '1'):
        environ['wsgi.url_scheme'] = 'https'
    else:
        environ['wsgi.url_scheme'] = 'http'

    headers_set = []
    headers_sent = []

    def write(data):
        if not headers_set:
             raise AssertionError("write() before start_response()")

        elif not headers_sent:
             # �ڷ�������֮ǰ���ȰѴ洢��ͷ���ͳ�ȥ
             status, response_headers = headers_sent[:] = headers_set
             sys.stdout.write('Status: %s\r\n' % status)
             for header in response_headers:
                 sys.stdout.write('%s: %s\r\n' % header)
             sys.stdout.write('\r\n')

        sys.stdout.write(data)
        sys.stdout.flush()

    def start_response(status, response_headers, exc_info=None):
        if exc_info:
            try:
                if headers_sent:
                    # �����ͷ�Ѿ������ͣ������׳��쳣
                    raise exc_info[0], exc_info[1], exc_info[2]
            finally:
                exc_info = None     # ������ѭ��
        elif headers_set:
            raise AssertionError("Headers already set!")

        headers_set[:] = [status, response_headers]
        return write

    result = application(environ, start_response)
    try:
        for data in result:
            if data:    # �����Ϣ���廹û�У����ܷ���ͷ
                write(data)
        if not headers_sent:
            write('')   # ����Ϊ�գ�����ͷ
    finally:
        if hasattr(result, 'close'):
            result.close()
```
��������ڴ����HTTP�ͻ�������ʱ���ǲ��û���ġ��ڴ����굱ǰ���ַ����Ż�������һ������Ҳ����ζ��Ӧ�ñ������Լ��Ļ��档���application���صĿɵ���������close()������������������е�result.close()����ô���������Ƿ��������������Ҫ����������������ͷ�application����Դ��


----------
#�м��
�ڷ���ˣ��м�����ֵ���һ��Ӧ�á�������Ӧ�öˣ��м�����ֵ���һ������ˡ��м���кܶ����ã�
>* ���ݲ�ͬ��URL������·�ɵ���ͬ��Ӧ��
>* ����ͬ��Ӧ�û��߿����ͬһ�������в�������
>* ͨ����������ת�������Ӧ��ʵ�ָ��ؾ����Զ�̴���
>* �������ݽ��к�ӹ�������Ӧ��xsl��ʽ���

�м���Ƚ����⣬��������server�˺�Ӧ�ö˵�˫�صĹ涨�������м�������кܶ�㣬ÿһ���м�������ṩ��ͬ�Ĺ��ܡ�

������һ���м�������ӡ�����м����Ӣ���β�ĳ�������ʽ��

```python 
from piglatin import piglatin

class LatinIter:

    """�������ת�����Ͱѿɵ�������ת��Ϊ�����

     Note that the "okayness" can change until the application yields its first non-empty string, so 'transform_ok' has to be a mutable truth value.
    """

    def __init__(self, result, transform_ok):
        if hasattr(result, 'close'):
            self.close = result.close
        self._next = iter(result).next
        self.transform_ok = transform_ok

    def __iter__(self):
        return self

    def next(self):
        if self.transform_ok:
            return piglatin(self._next())
        else:
            return self._next()

class Latinator:

    # Ĭ������²����������
    transform = False

    def __init__(self, application):
        self.application = application

    def __call__(self, environ, start_response):

        transform_ok = []

        def start_latin(status, response_headers, exc_info=None):

            # ����ok��־λ���Է�����һ���ظ��ĵ��á� 
            del transform_ok[:]

            for name, value in response_headers:
                if name.lower() == 'content-type' and value == 'text/plain':
                    transform_ok.append(True)
                    # Strip content-length if present, else it'll be wrong
                    response_headers = [(name, value)
                        for name, value in response_headers
                            if name.lower() != 'content-length'
                    ]
                    break

            write = start_response(status, response_headers, exc_info)

            if transform_ok:
                def write_latin(data):
                    write(piglatin(data))
                return write_latin
            else:
                return write

        return LatinIter(self.application(environ, start_latin), transform_ok)
# ��Latinator's����������foo_app, ʹ��ʾ����CGI�������ӡ�
from foo_app import foo_app
run_with_cgi(Latinator(foo_app))
```


----------


#����ѧϰ����
WSGI����������һ���涨�����������һֱ���ֵ�environ��������������ʹ����������룬url����������ȵȡ���Щ���Ƚ�ϸ�飬���������ܽ��ˡ�������һЩ�ȽϺõ�ѧϰ��Դ��

 1. [pep-0333�ٷ���վ](https://www.python.org/dev/peps/pep-0333/)
 2. [����WSGI�����](https://github.com/mainframer/PEP333-zh-CN)
 3. [http://blog.kenshinx.me/blog/wsgi-research/](http://blog.kenshinx.me/blog/wsgi-research/)
 4. [csdn����](http://blog.csdn.net/on_1y/article/details/18803563)