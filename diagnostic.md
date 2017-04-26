# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

  ```md
    it keeps the data normalized.  all data in one table can lead to a bad
    design for sure.
  ```

1.  Provide a database table structure and explain the Entity Relationship that
  describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
  (Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
  `Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
  join table with references to `Movies` and `Profiles`.

  ```md
  CREATE TABLE Profiles(
    id SERIAL PRIMARY KEY,
    given_name TEXT,
    surname TEXT
  );

  CREATE TABLE Movies(
    id SERIAL PRIMARY KEY,
    title TEXT,
    release_date  DATE,
    length FLOAT
    );

 CREATE TABLE Favorites(
    id SERIAL PRIMARY KEY,
    profile_id INTEGER REFERENCES Profiles(id),
    movie_id INTEGER REFERENCES Movies(id)
  );
  CREATE INDEX ON Favorites(profile_id);
  CREATE INDEX ON Favorites(movie_id);

  The Profiles table joins the Movie table to the Favorites table by have both
  the id fields of both tables as data

  ```

1.  For the above example, what needs to be added to the Model files?

  ```rb
  class Profile < ActiveRecord::Base
    has_many :movie, through: :favorite
    has_many :favorite, dependent: :destroy
  end
  ```

  ```rb
  class Movie < ActiveRecord::Base
    has_many :profile, through: :favorite
    has_many :favorite, dependent: :destroy
  end
  ```

  ```rb
  class Favorite < ActiveRecord::Base
    belongs_to :movie, inverse_of: :favorite
    belongs_to :profile, inverse_of: :favorite
  end
  ```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

  ```md
    The serializer allows you to filter certain data out of your response. The
    data still exists, it's just not shown in the response to users.
  ```

  ```rb
  class ProfileSerializer < ActiveModel::Serializer
    attributes :id, :given_name, :surname, :movie
  end
  ```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

  ```sh
    bin/rails generate scaffold favorite movie:references profile:references
  ```

1.  What is `Dependent: Destroy` and where/why would we use it?

  ```md
    It tells Rails what to do with the joined item when deleting an item. Simular to
    delete cascade on the db table
  ```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

  ```md
    # < Your Response Here >
  ```
