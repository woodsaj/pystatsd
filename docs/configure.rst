.. _configuring-chapter:

==================
Configuring Statsd
==================

It's easy to configure and use Statsd at runtime, but there are also two
shortcuts available.


Runtime
=======

If you are running the statsd_ server locally and on the default port,
it's extremely easy::

    from statsd import StatsClient

    statsd = StatsClient()
    statsd.incr('foo')

There are three arguments to configure your ``StatsClient`` instance.
They, and their defaults, are::

    from statsd import StatsClient

    statsd = StatsClient(host='localhost',
                         port=8125,
                         ipv6=False,
                         prefix=None,
                         maxudpsize=512)

``host`` is the host running the statsd server. If a hostname is given,
will lookup an IP v4 or v6 address based on the value of ``ipv6``. If an
address is given, the family should match ``ipv6``.

``port`` is the statsd server port. The default for both server and
client is ``8125``.

``ipv6`` enables the use of IPv6. Since the Etsy server defaults to v4
only, the default is ``False``.

``prefix`` helps distinguish multiple applications or environments using
the same statsd server. It will be prepended to all stats,
automatically. For example::

    from statsd import StatsClient

    foo_stats = StatsClient(prefix='foo')
    bar_stats = StatsClient(prefix='bar')

    foo_stats.incr('baz')
    bar_stats.incr('baz')

will produce two different stats, ``foo.baz`` and ``bar.baz``. Without
the ``prefix`` argument, or with the same ``prefix``, two
``StatsClient`` instances will update the same stats.

.. versionadded:: 2.0.3

``maxudpsize`` specifies the maximum packet size statsd will use. This is
an advanced options and should not be changed unless you know what you are
doing. Larger values then the default of 512 are generally deemed unsafe for use
on the internet. On a controlled local network or when the statsd server is
running on 127.0.0.1 larger values can decrease the number of UDP packets when
pipelining many metrics. Use with care!

.. versionchanged:: 3.2

``ipv6`` and 


In Django
=========

If you are using Statsd in a Django_ application, you can configure a
default ``StatsClient`` in the Django settings. All of these settings
are optional.

Here are the settings and their defaults::

    STATSD_HOST = 'localhost'
    STATSD_PORT = 8125
    STATSD_PREFIX = None
    STATSD_MAXUDPSIZE = 512

You can use the default ``StatsClient`` simply::

    from statsd.defaults.django import statsd

    statsd.incr('foo')


From the Environment
====================

Statsd isn't only useful in Django or on the web. A default instance
can also be configured via environment variables.

Here are the environment variables and their defaults::

    STATSD_HOST=localhost
    STATSD_PORT=8125
    STATSD_PREFIX=None
    STATSD_MAXUDPSIZE=512

and then in your Python application, you can simply do::

    from statsd.defaults.env import statsd

    statsd.incr('foo')

.. note::

    As of version 3.0, this default instance is always available,
    configured with the default values, unless overridden by the
    environment.

.. _statsd: https://github.com/etsy/statsd
.. _StatsD: https://github.com/etsy/statsd
.. _Django: https://www.djangoproject.com/
