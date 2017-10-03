# SqlAlchemy bundle

Support for SqlAlchemy in [applauncher](https://github.com/maxpowel/applauncher)

Install: pip install sqlalchemy_bundle

## Configuration example

```yml
entity_manager:
  #driver: "mysql+pymysql"
  driver: sqlite:///database.db
  username: {database_username}
  password: {database_password}
  hostname: {database_hostname}
  port: {database_port}
  database: {database_name}
```

## Defining your model

```python
from sqlalchemy import Column, Integer, String, ForeignKey
from sqlalchemy.orm import relationship
from sqlalchemy_bundle import Base

class User(Base):
    __tablename__ = 'user'
    id = Column(Integer, primary_key=True)
    name = Column(String(50))
    city = relationship("City")
    city_id = Column(Integer, ForeignKey("city.id"))
    

class City(Base):
    __tablename__ = 'city'
    id = Column(Integer, primary_key=True)
    name = Column(String(50))
```

I recommend to create a file called 'model.py' with your models. Then from your bundle
import this file. By doing this sqlalchemy will know all your models which is necessary
for create schema and foreign keys

## Creating the schema
```python
from sqlalchemy_bundle import EntityManager
import inject

em = inject.instance(EntityManager)
em.generate_schema()
```

## Querying the database

```python
from sqlalchemy_bundle import EntityManager
import inject

em = inject.instance(EntityManager)
with em.s as session:
    q = session.query(model.User).join(model.User.city)
    for i in q.all():
        print(i.name)
```

or if you prefer the real session object

```python
from sqlalchemy_bundle import EntityManager
import inject

em = inject.instance(EntityManager)
session = em.Session()
q = session.query(model.User).join(model.User.city)
for i in q.all():
    print(i.name)
session.close()
```

For more information about querying just check the SqlAlchemy
documentation

