.. index::
   single: Preamble
   
Preamble
========

The Team
------------------
The Fuchsia Team has a combined experience of 50 years as Web Developers and have used many Web Frameworks. Rita van den Huevel has worked with Laravel since its first stable release, Elise "Ellie" Porshel hacked away on Lithium for JSON services, Jack "Kramer" Peltonen built complex eCommerce systems with CodeIgniter, and Benjamin Anderson slaved away with Ruby on Rails as a go-to resource.

Consensus on Frameworks
------------------
Frameworks provide developers a set of commonly used libraries that accelerate development at the cost of performance, generally. We've summarized some of the strengths and weaknesses of Frameworks we've used. We wish to learn of the downfalls of these frameworks and improve upon them ourselves.

CodeIgniter
~~~~~~~~~~~~~~~~
CodeIgniter is an excellent Framework if you're migrating from PHP4 to a more up-to-date version. It does not support any form of Autoloading or Namespaces in its core as a limitation of PHP4. For us using more up-to-date versions of PHP, CodeIgniter was not among Rita's first choices when developing applications. The lack of supporting unit tests until a more recent version (which still isn't covered completely) simply stated to us developers that <strong>CodeIgniter was not built to support Test-Driven Development.</strong> Our applications on CodeIgniter can somewhat be tested, but never entirely. 

Summary: No autoloading, lack of namespaces, insufficient core unit tests, testing applications is near impossible.

Lithium (Li3)
~~~~~~~~~~~~~~~~
Lithium is a step-up from CodeIgniter. It is decently documented (could use a lot of improvements) and supports Unit Testing. This was a major decisive factor when Elise chose Lithium as her go-to framework initially. Its database layer is a little immature, lacking many elements including a HABTM (has and belongs to many) relationships. Libraries and components are not really decoupled -- causing dependency issues. There's a lack of support for a package manager like composer. Though Lithium has shown lots of promise in its development, it no longer suited Elise for medium-sized applications. The use of eval() and extract() has made the progression of the framework slow down rapidly.

Summary: Incomplete Database Layer, Components/Packages are not Decoupled, No Package Manager Support, eval(), extract().

Laravel
~~~~~~~~~~~~~~~~
Laravel is what CakePHP should have been. Its codebase is clean, not robust, and doesn't entirely try to copy the popular ruby framework - rails. Its database layer is mature, components are modularized well, the composer package manager is supported, and it's documentation is good... Really good. However, though it provides much of what is needed for developing mid to large sized applications, one of its major downfalls is that it uses Facades. 
Facades are dangerous, because they allow you to sprinkle dependencies framework-specific dependencies in one's code without much thought and without it being especially obvious (compared to Dependency Injection). Eloquent is also a major downfall. It's treated as a "god" class that does a lot of magic under the hood. This is good when everything is working properly, however, when you need to debug an issue with something that inherits/extends Eloquent -- it's near impossible to debug.

Summary: Facades, Eloquent - Difficult Debugging 