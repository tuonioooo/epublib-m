# epublib
Epublib is a java library for reading/writing/manipulating epub files.

[![Build Status](https://circleci.com/gh/positiondev/epublib.svg?style=shield)](https://circleci.com/gh/positiondev/epublib)
[![Maven Central: epublib](https://maven-badges.herokuapp.com/maven-central/com.positiondev.epublib/epublib-parent/badge.svg)](https://maven-badges.herokuapp.com/maven-central/com.positiondev.epublib/epublib-parent)

I consists of 2 parts: a core that reads/writes epub and a collection of tools.
The tools contain an epub cleanup tool, a tool to create epubs from html files, a tool to create an epub from an uncompress html file.
It also contains a swing-based epub viewer.
![Epublib viewer](http://www.siegmann.nl/wp-content/uploads/Alice%E2%80%99s-Adventures-in-Wonderland_2011-01-30_18-17-30.png)

The core runs both on android and a standard java environment. The tools run only on a standard java environment.

This means that reading/writing epub files works on Android.

## Command line examples

Set the author of an existing epub
	java -jar epublib-3.0-SNAPSHOT.one-jar.jar --in input.epub --out result.epub --author Tester,Joe

Set the cover image of an existing epub
	java -jar epublib-3.0-SNAPSHOT.one-jar.jar --in input.epub --out result.epub --cover-image my_cover.jpg

## Creating an epub programmatically

	package nl.siegmann.epublib.examples;

	import java.io.InputStream;
	import java.io.FileOutputStream;
	 
	import nl.siegmann.epublib.domain.Author;
	import nl.siegmann.epublib.domain.Book;
	import nl.siegmann.epublib.domain.Metadata;
	import nl.siegmann.epublib.domain.Resource;
	import nl.siegmann.epublib.domain.TOCReference;
	
	import nl.siegmann.epublib.epub.EpubWriter;
	 
	public class Translator {
	  private static InputStream getResource( String path ) {
	    return Translator.class.getResourceAsStream( path );
	  }
	
	  private static Resource getResource( String path, String href ) {
	    return new Resource( getResource( path ), href );
	  }
	
	  public static void main(String[] args) {
	    try {
	      // Create new Book
	      Book book = new Book();
	      Metadata metadata = book.getMetadata();
	       
	      // Set the title
	      metadata.addTitle("Epublib test book 1");
	       
	      // Add an Author
	      metadata.addAuthor(new Author("Joe", "Tester"));
	       
	      // Set cover image
	      book.setCoverImage(
	        getResource("/book1/test_cover.png", "cover.png") );
	       
	      // Add Chapter 1
	      book.addSection("Introduction",
	        getResource("/book1/chapter1.html", "chapter1.html") );
	       
	      // Add css file
	      book.getResources().add(
	        getResource("/book1/book1.css", "book1.css") );
	       
	      // Add Chapter 2
	      TOCReference chapter2 = book.addSection( "Second Chapter",
	        getResource("/book1/chapter2.html", "chapter2.html") );
	       
	      // Add image used by Chapter 2
	      book.getResources().add(
	        getResource("/book1/flowers_320x240.jpg", "flowers.jpg"));
	       
	      // Add Chapter2, Section 1
	      book.addSection(chapter2, "Chapter 2, section 1",
	        getResource("/book1/chapter2_1.html", "chapter2_1.html"));
	       
	      // Add Chapter 3
	      book.addSection("Conclusion",
	        getResource("/book1/chapter3.html", "chapter3.html"));
	       
	      // Create EpubWriter
	      EpubWriter epubWriter = new EpubWriter();
	       
	      // Write the Book as Epub
	      epubWriter.write(book, new FileOutputStream("test1_book1.epub"));
	    } catch (Exception e) {
	      e.printStackTrace();
	    }
	  }
	}
	
	
## Copyright

* Portions Copyright 2017 Position Development, LLC.
* Portions Copyright 2009-2016 Paul Siegmann (http://www.siegmann.nl/epublib/license)

epublib is free software: you can redistribute it and/or modify it under the terms of the GNU Lesser General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

epublib is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU Lesser General Public License for more details.

You should have received a copy of the GNU Lesser General Public License along with epublib. If not, see http://www.gnu.org/licenses/.
