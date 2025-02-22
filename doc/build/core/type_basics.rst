Column and Data Types
=====================

.. module:: sqlalchemy.types

SQLAlchemy provides abstractions for most common database data types,
and a mechanism for specifying your own custom data types.

The methods and attributes of type objects are rarely used directly.
Type objects are supplied to :class:`~sqlalchemy.schema.Table` definitions
and can be supplied as type hints to `functions` for occasions where
the database driver returns an incorrect type.

.. code-block:: pycon

  >>> users = Table('users', metadata,
  ...               Column('id', Integer, primary_key=True),
  ...               Column('login', String(32))
  ...              )

SQLAlchemy will use the ``Integer`` and ``String(32)`` type
information when issuing a ``CREATE TABLE`` statement and will use it
again when reading back rows ``SELECTed`` from the database.
Functions that accept a type (such as :func:`~sqlalchemy.schema.Column`) will
typically accept a type class or instance; ``Integer`` is equivalent
to ``Integer()`` with no construction arguments in this case.

.. _types_generic:

Generic Types
-------------

Generic types specify a column that can read, write and store a
particular type of Python data.  SQLAlchemy will choose the best
database column type available on the target database when issuing a
``CREATE TABLE`` statement.  For complete control over which column
type is emitted in ``CREATE TABLE``, such as ``VARCHAR`` see
:ref:`types_sqlstandard` and the other sections of this chapter.

.. autoclass:: BigInteger
   :members:

.. autoclass:: Boolean
   :members:

.. autoclass:: Date
   :members:

.. autoclass:: DateTime
   :members:

.. autoclass:: Enum
  :members: __init__, create, drop

.. autoclass:: Double
   :members:

.. autoclass:: Float
  :members:

.. autoclass:: Integer
  :members:

.. autoclass:: Interval
  :members:

.. autoclass:: LargeBinary
  :members:

.. autoclass:: MatchType
  :members:

.. autoclass:: Numeric
  :members:

.. autoclass:: PickleType
  :members:

.. autoclass:: SchemaType
  :members:
  :undoc-members:

.. autoclass:: SmallInteger
  :members:

.. autoclass:: String
   :members:

.. autoclass:: Text
   :members:

.. autoclass:: Time
  :members:

.. autoclass:: Unicode
  :members:

.. autoclass:: UnicodeText
   :members:

.. autoclass:: Uuid
  :members:

.. _types_sqlstandard:

SQL Standard and Multiple Vendor Types
--------------------------------------

This category of types refers to types that are either part of the
SQL standard, or are potentially found within a subset of database backends.
Unlike the "generic" types, the SQL standard/multi-vendor types have **no**
guarantee of working on all backends, and will only work on those backends
that explicitly support them by name.  That is, the type will always emit
its exact name in DDL with ``CREATE TABLE`` is issued.


.. autoclass:: ARRAY
   :members:

.. autoclass:: BIGINT


.. autoclass:: BINARY


.. autoclass:: BLOB


.. autoclass:: BOOLEAN


.. autoclass:: CHAR


.. autoclass:: CLOB


.. autoclass:: DATE


.. autoclass:: DATETIME


.. autoclass:: DECIMAL

.. autoclass:: DOUBLE

.. autoclass:: DOUBLE_PRECISION

.. autoclass:: FLOAT


.. autoclass:: INT

.. autoclass:: JSON
    :members:


.. autoclass:: sqlalchemy.types.INTEGER


.. autoclass:: NCHAR


.. autoclass:: NVARCHAR


.. autoclass:: NUMERIC


.. autoclass:: REAL


.. autoclass:: SMALLINT


.. autoclass:: TEXT


.. autoclass:: TIME


.. autoclass:: TIMESTAMP
    :members:


.. autoclass:: UUID

.. autoclass:: VARBINARY


.. autoclass:: VARCHAR


.. _types_vendor:

Vendor-Specific Types
---------------------

Database-specific types are also available for import from each
database's dialect module. See the :ref:`dialect_toplevel`
reference for the database you're interested in.

For example, MySQL has a ``BIGINT`` type and PostgreSQL has an
``INET`` type.  To use these, import them from the module explicitly::

    from sqlalchemy.dialects import mysql

    table = Table('foo', metadata,
        Column('id', mysql.BIGINT),
        Column('enumerates', mysql.ENUM('a', 'b', 'c'))
    )

Or some PostgreSQL types::

    from sqlalchemy.dialects import postgresql

    table = Table('foo', metadata,
        Column('ipaddress', postgresql.INET),
        Column('elements', postgresql.ARRAY(String))
    )

Each dialect provides the full set of database types supported by
that backend within its own module, so they may all be used
against the module directly without the need to differentiate between
which types are specific to that backend or not::

    from sqlalchemy.dialects import postgresql

    t = Table('mytable', metadata,
               Column('id', postgresql.INTEGER, primary_key=True),
               Column('name', postgresql.VARCHAR(300)),
               Column('inetaddr', postgresql.INET)
    )

Where above, the INTEGER and VARCHAR types are ultimately from
sqlalchemy.types, and INET is specific to the PostgreSQL dialect.

Some dialect level types have the same name as the SQL standard type,
but also provide additional arguments.  For example, MySQL implements
the full range of character and string types including additional arguments
such as `collation` and `charset`::

    from sqlalchemy.dialects.mysql import VARCHAR, TEXT

    table = Table('foo', metadata_obj,
        Column('col1', VARCHAR(200, collation='binary')),
        Column('col2', TEXT(charset='latin1'))
    )

