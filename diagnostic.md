# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

  ```md
    # To create a many to many relationship.
  ```

1.  Provide a database table structure and explain the Entity Relationship that
  describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
  (Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
  `Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
  join table with references to `Movies` and `Profiles`.

  ```md
    # Profiles is a main
      id primary key | given_name | surname | email

      Movies is a main
      id primary key | title | release_date | length

      Favorites is a join (profiles have movies through favorites and vice versa)
      id primary key | profile_id | movie_id
  ```

1.  For the above example, what needs to be added to the Model files?

  ```rb
  class Profile < ActiveRecord::Base
    has_many :favorites through movies
    has_many :movies

    validates :movie, presence: true
  end
  ```

  ```rb
  class Movie < ActiveRecord::Base
    has_many :favorites through profiles
    has_many :profiles

    validates :profile, presence: true
  end
  ```

  ```rb
  class Favorite < ActiveRecord::Base
    belongs_to :profiles
    belongs_to :movies
  end
  ```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

  ```md
    # Serializers filter and reshape data.
  ```

  ```rb
  class ProfileSerializer < ActiveModel::Serializer
    attributes :given_name, :surname, :email
  end
  ```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

  ```sh
    # bin/rails generate scaffold favorites profile:references movie:references
  ```

1.  What is `Dependent: Destroy` and where/why would we use it?

  ```md
    # it destroys a favorite in order to delete a movie or profile. We would use it in the models.
  ```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

  ```md
    # facebook: a user has many posts, but once users comment on posts, then the posts have many users.
  ```
