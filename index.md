---
layout: page
title: Introduction
header: SORM
tagline: A functional boilerplate-free Scala ORM
comments: false
keywords: 
  - scalable
  - akka
  - actors
  - concurrency
  - multithreading
  - connection pooling
  - case class
  - functional programming
  - high level
---

SORM is a Scala ORM-framework designed to eliminate boilerplate code and solve the problems of scalability with a high level abstraction and a functional programming style.

##Features

* __Complete abstraction from relational concepts__. You work with case classes, collections and other standard Scala data types instead of tables, rows, foreign keys and relations.
* __Complete separation of domain model from persistence layer__. There are no annotations, special types or any other dependencies on persistence layer present in model declaration. This house is clear!
* __An intuitive and centralized connection-agnostic API__. No tangled implicit constructions polluting your namespace and functionality scattered across multiple components. No manual management of connections.
* __Concurrency__. A single SORM instance can safely be used across multiple threads and seamlessly integrates into actor-based concurrent systems, like Akka.
* __Integrated connection pooling__. Scalable just by setting a "poolSize" parameter.
* __Automated schema generation__.

##A Quick Intro

Following is a complete example. No extra code is required for it to work.

{% highlight scala %}
// Declare a model:
case class Artist( name : String, genres : Set[Genre] )
case class Genre( name : String ) 

// Initialize SORM, automatically generating schema:
import sorm._
object Db extends Instance(
  entities = Set( Entity[Artist](), Entity[Genre]() ),
  url = "jdbc:h2:mem:test"
)

// Store values in the db:
val metal = Db.save( Genre("Metal") )
val rock = Db.save( Genre("Rock") )
Db.save( Artist("Metallica", Set(metal, rock) ) )
Db.save( Artist("Dire Straits", Set(rock) ) )

// Retrieve values from the db:
// Option[Artist with Persisted]:
val metallica = Db.query[Artist].whereEqual("name", "Metallica").fetchOne() 
// Stream[Artist with Persisted]:
val rockArtists = Db.query[Artist].whereEqual("genres.item.name", "Rock").fetch() 
{% endhighlight %}

##Installation and Latest Release Info
Latest release info and installation instructions are provided at the project's [GitHub page](https://github.com/sorm/sorm#readme).

##Learn More
You can see how SORM compares to Slick [here](/SORM-vs-Slick.html) or continue on to a more comprehensive [Tutorial](/Tutorial.html). For detailed documentation please visit [this page](/Documentation.html) or learn the [API](/sorm/current/) (you're really interested in the contents of a plain `sorm._` package only).

##Support and Issues
If you have a question concerning how to use SORM, post it on [StackOverflow](http://stackoverflow.com/questions/tagged/sorm) under a tag "sorm".

If you come across any issue, have a suggestion or a feature request, please, post it on the project's [Issue Tracker](https://github.com/sorm/sorm/issues).
