---
title: "Example Code Blocks"
layout: post
author: ""
publish_date: "6/12/2020"
last_modified: "6/13/2020"
---

This theme by default uses [Rogue](http://rouge.jneen.net/) for syntax highlighting.

Here is some example code blocks from [99 Bottles of Beer](http://www.99-bottles-of-beer.net) collection. I find these are better at displaying code examples then your normal hello world program.

# C

~~~c
/*  The 99 Bottles of Beer Linux Kernel Module v1.1
 *  (supports multiple driver instances)
 *
 *  by Stefan Scheler <sts[at]synflood[dot]de>
 *  August 2nd, 2005 - Ernstthal, Germany
 *
 *  Usage:
 *  1) compile the module
 *  2) create the device: mknod /dev/bottles c 240 0
 *  3) load the module: insmod bottles.ko
 *  4) print the song with: cat /dev/bottles
 */

#include <linux/fs.h>
#include <linux/version.h>
#include <linux/module.h>
#include <linux/init.h>
#include <asm/uaccess.h>

#define DRIVER_MAJOR 240
#define BUFFERSIZE 160
#define PLURALS(b) (b>1)?"s":""

MODULE_AUTHOR("Stefan Scheler <sts[at]synflood[dot]de>");
MODULE_LICENSE("GPL");
MODULE_DESCRIPTION("The 99 Bottles of Beer Linux Kernel Module");
MODULE_SUPPORTED_DEVICE("Bottle of Beer");

struct _instance_data {
    int bytes_avail, bytes_sent, bottles;
    char buf[BUFFERSIZE];
};

static void fill_buffer(char *buf, int b) {
    char line[BUFFERSIZE/2];
    if (b>0) {
        sprintf(buf, "%d bottle%s of beer on the wall, %d bottle%s of beer.\n" \
                     "Take one down and pass it around, ", b, PLURALS(b), b, PLURALS(b));
        if (b==1)
            strcat(buf, "no more bottles of beer on the wall.\n");
        else {
            sprintf(line, "%d bottle%s of beer on the wall.\n", b-1, PLURALS(b-1));
            strcat(buf, line);
        }
    } else {
        sprintf(buf, "No more bottles of beer on the wall, no more bottles of beer.\n" \
                     "Go to the store and buy some more, 99 bottles of beer on the wall.\n");
    }
}

static ssize_t driver_read(struct file *instance, char *userbuffer, size_t count, loff_t *offset) {
    struct _instance_data *iptr = (struct _instance_data *)instance->private_data;

    int to_copy;
    int not_copied;

refillbuffer:
    fill_buffer(iptr->buf, iptr->bottles);
    iptr->bytes_avail = strlen(iptr->buf)+1;
    to_copy = iptr->bytes_avail-iptr->bytes_sent;

    if (to_copy>0) {
        if (to_copy> count) to_copy=count;
        not_copied=copy_to_user(userbuffer, iptr->buf+iptr->bytes_sent, to_copy);
        iptr->bytes_sent += to_copy-not_copied;
        return (to_copy-not_copied);
    }

    if ((to_copy==0) && (iptr->bottles>0)) {
        iptr->bytes_sent=0; 
        iptr->bottles--;
        goto refillbuffer;
    }

    return 0;
}

int driver_open(struct inode *devicefile, struct file *instance)  {
    struct _instance_data *iptr;
    iptr = (struct _instance_data *)kmalloc(sizeof(struct _instance_data), GFP_KERNEL);
    
    if (!iptr) 
        return -1;

    iptr->bytes_sent = 0;
    iptr->bottles = 99;
    instance->private_data = iptr;

    return 0;
}

int driver_close(struct inode *devicefile, struct file *instance)  {
    if (instance->private_data)
        kfree(instance->private_data);
    return 0;
}

static struct file_operations fops = {
    .owner   = THIS_MODULE,
    .open    = driver_open,
    .release = driver_close,
    .read    = driver_read,
};

static int __init __init_module(void) {
    if(register_chrdev(DRIVER_MAJOR, "99 Bottles of Beer", &fops) == 0) 
        return 0;
    return -EIO;
}

static void __exit __cleanup_module(void) {
    unregister_chrdev(DRIVER_MAJOR, "99 Bottles of Beer");
}

module_init(__init_module);
module_exit(__cleanup_module);
~~~

# C#

~~~c#
/// A short and sweet C# 3.5 / LINQ implementation of 99 Bottles of Beer
/// Jeff Dietrich, jd@discordant.org - October 26, 2007

using System;
using System.Linq;
using System.Text;

namespace NinetyNineBottles
{
  class Beer
  {
    static void Main(string[] args)
    {
        StringBuilder beerLyric = new StringBuilder();
        string nl = System.Environment.NewLine;

        var beers =
            (from n in Enumerable.Range(0, 100)
             select new { 
               Say =  n == 0 ? "No more bottles" : 
                     (n == 1 ? "1 bottle" : n.ToString() + " bottles"),
               Next = n == 1 ? "no more bottles" : 
                     (n == 0 ? "99 bottles" : 
                     (n == 2 ? "1 bottle" : n.ToString() + " bottles")),
               Action = n == 0 ? "Go to the store and buy some more" : 
                                 "Take one down and pass it around"
             }).Reverse();

        foreach (var beer in beers)
        {
            beerLyric.AppendFormat("{0} of beer on the wall, {1} of beer.{2}",
                                    beer.Say, beer.Say.ToLower(), nl);
            beerLyric.AppendFormat("{0}, {1} of beer on the wall.{2}", 
                                    beer.Action, beer.Next, nl);
            beerLyric.AppendLine();
        }
        Console.WriteLine(beerLyric.ToString());
        Console.ReadLine();
    }
  }
}
~~~

# C++
~~~cpp
// -*- Mode: C++ -*-
// Oct 2011 by Henrik Theiling
// Licence: BSD
// Checked to work with g++-4.6.2 -std=c++0x


// Base char type for the strings: you can very well use char32_t, but unfortunately,
// the compiler will not show it as a character in its messages, so to demonstrate
// 'readable' type names, we use char.
typedef char Char;


// If
template<bool Cond, typename Then, typename Else>
struct If {
    typedef Then Type;
};
template<typename Then, typename Else>
struct If<false, Then, Else> {
    typedef Else Type;
};


// String
template<Char... string>
struct String {
    // The funny thing is that we do not store the string at all:
    // It is only stored in the type itself.  If you want a
    // C string, use 'CArray<char,...>::value'.
};


// Conversion to C values:
template<typename Elem, typename String>
struct CArray;

template<typename Elem, Char... string>
struct CArray< Elem, String<string...> > {
    enum { size = sizeof...(string) };
    static Elem const value[size + 1];
};
template<typename Elem, Char... string>
Elem const CArray<Elem, String<string...> >::value[] = { string..., '{CODE}' };


// Edit a string, list style:
template<typename String>
struct Car;

template<Char first, Char... rest>
struct Car< String<first, rest... > > {
    enum { value = first };
};


template<typename String>
struct Cdr;

template<Char first, Char... rest>
struct Cdr< String<first, rest... > > {
    typedef String<rest...> Type;
};


template<typename String, int n>
struct Nth
    : public Nth< typename Cdr<String>::Type, n - 1>
{};

template<typename String>
struct Nth< String, 0 >
    : public Car< String >
{};


template<Char car, typename cdr>
struct Conc;

template<Char first, Char... rest>
struct Conc< first, String<rest...> >
{
    typedef String<first, rest...> Type;
};


// Concatenate strings:
template<typename... Strings>
struct Concat;

template<typename String>
struct Concat<String>
{
    typedef String Type;
};

template<
    Char... part1,
    Char... part2>
struct Concat< String<part1...>,
               String<part2...> >
{
    typedef String<part1..., part2...> Type;
};

template<typename First, typename Second, typename... Rest>
struct Concat<First,Second,Rest...>
    : public Concat< typename Concat<First,Second>::Type, Rest...>
{};


// Join: similar to Concat, but adds a string in between each element.
template<typename Separator, typename... Strings>
struct Join;

template<typename Separator>
struct Join<Separator>
{
    typedef String<> Type;
};

template<typename Separator, typename Single>
struct Join<Separator,Single>
{
    typedef Single Type;
};

template<typename Separator, typename First, typename... Rest>
struct Join<Separator,First,Rest...>
    : public Concat<
        First,
        Separator,
        typename Join<Separator, Rest...>::Type >
{};


// Stringify integers:
enum class Show0 { no, yes };

template<unsigned long long num, Show0 show0 = Show0::yes>
struct Unsigned2String {
    typedef
    typename Concat<
        typename Unsigned2String< (num / 10), Show0::no >::Type,
        String<('0' + (num % 10))>
        >::Type
    Type;
};

template<>
struct Unsigned2String<0, Show0::yes> {
    typedef String<'0'> Type;
};

template<>
struct Unsigned2String<0, Show0::no> {
    typedef String<> Type;
};


// Convert a single letter to upper case:
// (FIXME: add Unicode support)
template<Char orig>
struct ToUpper {
    enum { value = orig };
};
template<> struct ToUpper<'a'> { enum { value = 'A' }; };
template<> struct ToUpper<'b'> { enum { value = 'B' }; };
template<> struct ToUpper<'c'> { enum { value = 'C' }; };
template<> struct ToUpper<'d'> { enum { value = 'D' }; };
template<> struct ToUpper<'e'> { enum { value = 'E' }; };
template<> struct ToUpper<'f'> { enum { value = 'F' }; };
template<> struct ToUpper<'g'> { enum { value = 'G' }; };
template<> struct ToUpper<'h'> { enum { value = 'H' }; };
template<> struct ToUpper<'i'> { enum { value = 'I' }; };
template<> struct ToUpper<'j'> { enum { value = 'J' }; };
template<> struct ToUpper<'k'> { enum { value = 'K' }; };
template<> struct ToUpper<'l'> { enum { value = 'L' }; };
template<> struct ToUpper<'m'> { enum { value = 'M' }; };
template<> struct ToUpper<'n'> { enum { value = 'N' }; };
template<> struct ToUpper<'o'> { enum { value = 'O' }; };
template<> struct ToUpper<'p'> { enum { value = 'P' }; };
template<> struct ToUpper<'q'> { enum { value = 'Q' }; };
template<> struct ToUpper<'r'> { enum { value = 'R' }; };
template<> struct ToUpper<'s'> { enum { value = 'S' }; };
template<> struct ToUpper<'t'> { enum { value = 'T' }; };
template<> struct ToUpper<'u'> { enum { value = 'U' }; };
template<> struct ToUpper<'v'> { enum { value = 'V' }; };
template<> struct ToUpper<'w'> { enum { value = 'W' }; };
template<> struct ToUpper<'x'> { enum { value = 'X' }; };
template<> struct ToUpper<'y'> { enum { value = 'Y' }; };
template<> struct ToUpper<'z'> { enum { value = 'Z' }; };


// Change the first character of a string to upper case:
template<typename String>
struct Capitalise
    : public Conc<
        ToUpper< Car<String>::value >::value,
        typename Cdr<String>::Type >
{};

template<>
struct Capitalise< String<> >
{
    typedef String<> Type;
};


// Sentence components:
typedef String<' '>                      _SPACE_;
typedef String<'\n'>                     _LINEBREAK_;
typedef String<'.'>                      _PERIOD_;
typedef String<',',' '>                  _COMMA_SPACE_;

typedef String<'a','n','d'>              _and_;
typedef String<'a','r','o','u','n','d'>  _around_;
typedef String<'b','e','e','r'>          _beer_;
typedef String<'b','o','t','t','l','e'>  _bottle_;
typedef String<'b','u','y'>              _buy_;
typedef String<'d','o','w','n'>          _down_;
typedef String<'g','o'>                  _go_;
typedef String<'i','t'>                  _it_;
typedef String<'m','o','r','e'>          _more_;
typedef String<'n','o'>                  _no_;
typedef String<'o','f'>                  _of_;
typedef String<'o','n','e'>              _one_;
typedef String<'o','n'>                  _on_;
typedef String<'p','a','s','s'>          _pass_;
typedef String<'s','o','m','e'>          _some_;
typedef String<'s','t','o','r','e'>      _store_;
typedef String<'t','a','k','e'>          _take_;
typedef String<'t','h','e'>              _the_;
typedef String<'t','o'>                  _to_;
typedef String<'w','a','l','l'>          _wall_;


// Pluralisation: the default is to add an 's':
// Irregular plurals can be added by specilising PluralOf<>.
// But we don't need that.
template<typename Singular>
struct PluralOf
    : public Concat<Singular, String<'s'> >
{};


// Singular or Plural depending on a number:
template<unsigned count, typename Singular>
struct SingularOrPlural
    : public If<
        (count == 1),
        Singular,
        typename PluralOf<Singular>::Type >
{};


// Add a number in front of a noun and adjust the pluralisation
// of the noun accordingly:
template<unsigned count, typename Singular>
struct CountNoun
    : public Concat<
        typename Unsigned2String<count>::Type,
        _SPACE_,
        typename SingularOrPlural <count,Singular>::Type >
{};


// 'N Units of beer':
template<int count, typename Unit>
struct NUnitsOfBeer
    : public Join<
        _SPACE_,
        typename CountNoun<count,Unit>::Type, _of_, _beer_>
{};
template<typename Unit>
struct NUnitsOfBeer<0,Unit>
    : public Join<
        _SPACE_,
        _no_, _more_, typename PluralOf<Unit>::Type, _of_, _beer_>
{};


// 'N Units of beer on the wall':
template<int count, typename Unit>
struct NUnitsOfBeerOnTheWall
    : public Join<
        _SPACE_,
        typename NUnitsOfBeer<count,Unit>::Type, _on_, _the_, _wall_ >
{};


// Last line of verse:
template<int count, int totalCount, typename Unit>
struct NMinus1UnitsOfBeerOnTheWall
    : public Concat<
        typename Join<
            _SPACE_,
            _take_, _one_, _down_, _and_, _pass_, _it_, _around_
        >::Type, _COMMA_SPACE_,
        typename NUnitsOfBeerOnTheWall<count - 1,Unit>::Type
    >
{};
template<int totalCount, typename Unit>
struct NMinus1UnitsOfBeerOnTheWall<0,totalCount,Unit>
    : public Concat<
        typename Join<
            _SPACE_,
            _go_, _to_, _the_, _store_, _and_, _buy_, _some_, _more_
        >::Type, _COMMA_SPACE_,
        typename NUnitsOfBeerOnTheWall<totalCount, Unit>::Type
    >
{};


// One Verse:
template<int count, int totalCount, typename Unit>
struct Verse
{
    typedef
    typename Concat<
        typename NUnitsOfBeerOnTheWall<count,Unit>::Type, _COMMA_SPACE_,
        typename NUnitsOfBeer<count,Unit>::Type, _PERIOD_
        >::Type
    Line1;

    typedef
    typename Concat<
        typename NMinus1UnitsOfBeerOnTheWall<count,totalCount,Unit>::Type, _PERIOD_
        >::Type
    Line2;

    typedef
    typename Concat<
        typename Capitalise<Line1>::Type, _LINEBREAK_,
        typename Capitalise<Line2>::Type, _LINEBREAK_
        >::Type
    Type;
};


// Bottles:
template<int count, int totalCount, typename Unit>
struct MakeSong
{
    typedef
    typename Concat<
        typename Verse<count, totalCount, Unit>::Type,
        _LINEBREAK_,
        typename MakeSong<count - 1, totalCount, Unit>::Type
        >::Type
    Type;
};
template<int totalCount, typename Unit>
struct MakeSong<0,totalCount,Unit>
    : public Verse<0, totalCount, Unit>
{};


// Make the song:
template<int count, typename Unit>
struct Song: public MakeSong<count,count,Unit>
{};

//////////////////////////////////////////////////////////////////////
// Usage:

typedef Song<99,_bottle_>::Type TheBeerSong;

//////////////////////////////////////////////////////////////////////
// Usage 1: Let the compiler sing the song

#if 1

static int i= TheBeerSong();
    // cause an error => the compiler prints the type name...

#endif

//////////////////////////////////////////////////////////////////////
// Usage 2: Print the song

#if 0

#include <stdio.h>

int main(void)
{
    puts(CArray<char,TheBeerSong>::value);
    return 0;
}

#endif
~~~

# Go

~~~go
/*  The 99 Bottles of Beer in the Go Proramming Language
 *
 *  Marcos Vazquez
 *  mvazquez[at]tamps.cinvestav[dot]mx
 *  November 10, 2010
 *
 */

package main

import "fmt"

var bottlesOfBeer int = 99;

func takeOneBeer(ch chan bool) {
    bottlesOfBeer--;
    go singTheSong(ch)
}

func singTheSong(ch chan bool) {
    if bottlesOfBeer > 1 {
        fmt.Printf("%d bottles of beer on the wall, %d bottles of beer.\n", 
        bottlesOfBeer, bottlesOfBeer)
        fmt.Println("Take one down and pass it around, 97 bottles of beer on the wall.\n")
        go takeOneBeer(ch)
    } else if bottlesOfBeer == 1 {
        fmt.Println("1 bottle of beer on the wall, 1 bottle of beer.")
        fmt.Println("Take one down and pass it around, no more bottles of beer on the wall.\n")
        go takeOneBeer(ch)
    } else {
        fmt.Println("No more bottles of beer on the wall, no more bottles of beer.")
        fmt.Println("Go to the store and buy some more, 99 bottles of beer on the wall.")
        ch <- true
    }
}

func main() {
    ch := make(chan bool)
    go singTheSong(ch)
    <- ch
}
~~~

# Java

~~~java
/* the main method instantiates drinkers and directs them to
   a newly-instantiated wall of beer.
*/
public class Sing99Bob {
  public static void main(String[] args) {
    ((NeedToFindBeer)(new Drinkers())).pointAtBeer(new WallOfBeer());
  }
}

  interface NeedToFindBeer { void pointAtBeer(WallOfBeer wob); }

  /* an instance of Drinkers, after being pointed at a wall containing
     beer, will periodically take one down.  The singing is driven by
     the taking of the beer.
  */
  class Drinkers extends Thread implements NeedToFindBeer {
    static final int drinkRate = 2;
    WallOfBeer ourBeer;
    public void run() {
      while (ourBeer.takeOne()>0) {
       try { Thread.sleep(drinkRate*1000); } catch (InterruptedException ignore) {}
      }
    }
    public void pointAtBeer(WallOfBeer wob) {
      ourBeer = wob;
      this.start();
    }
  }

  interface Countable { int howMany(); }

  /* an instance of a WallOfBeer will maintain a count of the
     beer bottles.  The wall has an associated Narrator who
     reports each time the number of bottles is changed.
  */
  class WallOfBeer implements Countable {
    static final int full = 99;
    int count = 0;
    WallWatcher ww;
    {
      ww = new Narrator();
      putSome(full);
    }
    void putSome(int some) {
      count += some;
      if (ww!=null) ww.wallEvent(this);
    }
    int takeOne() {
      count--;
      if (ww!=null) ww.wallEvent(this);
      return count;
    }
    public int howMany() { return count; }
  }

  interface WallWatcher { void wallEvent(Countable wob); }

  /* an instance of Narrator sings the verse each time it is
     notified that something has happened to the wall.  This
     narrator attempts to be grammatical, and uses words for
     the bottle count (singers sing words not numerals). The
     singing is paced, not merely lump-dumped to sysout.
  */
  class Narrator implements WallWatcher {
    static final int singspeed = 500;
    public void wallEvent(Countable wob) {
      int b = wob.howMany();
      try {
        System.out.print(bob(b, true)+onwall+", ");
        Thread.sleep(singspeed);
        System.out.print(bob(b, false)+".\n");
        Thread.sleep(singspeed);
        System.out.print((b>0)?takedown:gostore);
        Thread.sleep(singspeed);
        System.out.print((b>0)?bob(b-1, false):bob(WallOfBeer.full, false));
        System.out.print(onwall+".\n");
      } catch (InterruptedException ignore) {}
    }
    static final String bob(int i, boolean c) {
      String word = word(i);
      if (c) word = (word.substring(0, 1).toUpperCase())+word.substring(1);
      return word+" bottle"+((i==1)?"":"s")+" of beer";
    }
    static final String word(int i) {
      if (i==0) return "no more";
      if (i<20) return units[i];
      return tens[i/10]+((units[i%10]=="")?"":"-"+units[i%10]);
    }
    static final String
      onwall = " on the wall",
      takedown = "Take one down and pass it around, ",
      gostore = "Go to the store, get some more, ";
    static final String[]
      tens = {"", "", "twenty", "thirty", "forty",
              "fifty", "sixty", "seventy", "eighty", "ninety"},
      units = {"", "one", "two", "three", "four", "five", "six", "seven",
               "eight", "nine", "ten", "eleven", "twelve", "thirteen", "fourteen",
               "fifteen", "sixteen", "seventeen", "eighteen", "nineteen"};
  }
~~~

# JavaScript

~~~javascript
/**
 * @projectDescription Javascript snippet to generate the lyrics of the song "99 Bottles".
 * Copyright (c) 2008 Ariel Flesler - aflesler(at)gmail(dot)com | http://flesler.blogspot.com
 * Date: 3/27/2008
 * @author Ariel Flesler
 * --This script follows the standard specified by scriptDoc: http://scriptdoc.org/
 */

/**
 * @classDescription This class generates the lyrics of the song "99 Bottles".
 * @return { Song }	Returns a new Song Object.
 * @constructor
 */
var Song = function(){};

//add methods to the prototype, to affect the instances of the class Song
Song.prototype = {
	/**
	 * Maps an array of items using a function.
	 * @param { Array } src Source array whose items will be mapped.
	 * @param { Function } fn Function that receives each item & index and returns the mapped item.
	 * @return { Array } The mapped array.
	 * @method
	 * http://developer.mozilla.org/en/docs/Core_JavaScript_1.5_Reference:Global_Objects:Array:map
	 */
	map: function( src, fn ){
		var 
			mapped = [ ], //will hold the mapped items
			pos = src.length; //holds the actual index

		//do a reversed loop as we know the last index
		while( pos-- )
			mapped[pos] = fn.call( this, src[pos], pos );
		
		return mapped;
	},
	/**
	 * @param { Number } amount Specifies the # of bottle.
	 * @return { String } The related text.
	 * @method
	 */
	bottle:function( left ){
		switch( left ){
			case 0: return 'no more bottles';
			case 1: return '1 bottle';
			default: return left + ' bottles';
		}
	},
	/**
	 * @param { Number } amount The amount of bottles to buy.
	 * @method
	 */
	buy:function( amount ){
		this.bottles = Array(amount+1);
	},
	/**
	 * @param { String } separator String to separate each line.
	 * @return { String } Lyrics of the song.
	 * @method
	 */
	sing:function( separator ){
		var lines =  this.map( this.bottles, function( bottle, left ){
			
			bottle = this.bottle( left );//the text for this bottle, and then the next one.
			var nextBottle = left - 1 + ( left ? 0 : this.bottles.length );
			nextBottle = this.bottle( nextBottle );
			
			return this.apply( this.line1, bottle ) + separator +					
					this.apply( left ? this.line2a : this.line2b, nextBottle );
		});
		
		return lines.reverse().join( separator+separator );
	},
	caseRegex: /%([ul])/g, //matches %u or %l
	/**
	 * @param { String } template String template of the line.
	 * @param { String } bottle The text for this bottle.
	 * @return { String } The parsed template.
	 * @method
	 */
	apply:function( template, bottle ){
		return template.replace( this.caseRegex, function( all, kase ){
			return kase == 'l' ? bottle //lowercase
				: bottle.charAt(0).toUpperCase() + bottle.slice(1); //uppercase
		});	
	},
	line1: '%u of beer on the wall, %l of beer.',
	line2a: 'Take one down and pass it around, %l of beer on the wall.',
	line2b: 'Go to the store and buy some more, %l of beer on the wall.'
};

var bottlesSong = new Song();
bottlesSong.buy( 99 );

var lyrics = bottlesSong.sing( '<br />' );

document.body.innerHTML = lyrics;
~~~

# Lua

~~~lua
-- Lua 99 Bottles of Beer
-- by Philippe Lhoste <PhiLho@GMX.net> http://jove.prohosting.com/~philho/

function PrintBottleNumber(n)
  local bs
  if n == 0 then
    bs = "No more bottles"
  elseif n == 1 then
    bs = "One bottle"
  else
    bs = n .. " bottles"
  end
  return bs .. " of beer"
end

for bn = 99, 1, -1 do
  write(PrintBottleNumber(bn), " on the wall, \n")
  write(PrintBottleNumber(bn), "\n")
  write("Take one down and pass it around,\n")
  write(PrintBottleNumber(bn-1), " on the wall, \n\n")
end
write("No more bottles of beer on the wall,\nNo more bottles of beer\n")
write("Go to the store, buy some more!\n")
~~~

# MySQL

~~~sql
--
-- 99 Bottles of Beer
-- MySQL version
-- Works in mysql query browser for MySQL 4.1 and above
---
-- Based on Chris Farmer's Oracle version
-- 
SELECT
    CASE (a.tens * 10 + b.units)
        WHEN 0 THEN
              CONCAT('No more bottles of beer on the wall, no more bottles of beer. ' ,
                     'Go to the store and buy some more, 99 bottles of beer on the wall.')
        ELSE
               CONCAT((a.tens * 10 + b.units) , 
                  ' bottle', IF((a.tens * 10 + b.units)!=1, 's', ''),
                  ' of beer on the wall, ' ,
                  (a.tens * 10 + b.units),   
                  ' bottle', IF((a.tens * 10 + b.units)!=1, 's', ''),' of beer. ' ,
                  'Take one down and pass it around, ' ,
                  ((a.tens * 10 + b.units)-1) , 
		' bottle', IF(((a.tens * 10 + b.units)-1)!=1, 's', ''), 
                  ' of beer on the wall.')
        END as verse
 FROM 
    (select 0 as tens
	union select 1  
	union select 2  
	union select 3  
	union select 4  
	union select 5  
	union select 6  
	union select 7 
	union select 8  
	union select 9  ) as a
CROSS JOIN
    (select 0 as units
	union select 1   
	union select 2   
	union select 3  
	union select 4  
    	union select 5  
	union select 6  
	union select 7  
	union select 8  
	union select 9  ) as b
ORDER BY a.tens DESC, b.units DESC;
~~~

# PHP

~~~php
<?
	/* CONFIG FROM URL STRING, DEFAULT TO 99 BOTTLES OF BEER ON THE WALL*/
	if($_REQUEST['bottleNumber']) { $bottleNumber = $_REQUEST['bottleNumber']; } else { $bottleNumber =
99; }
	if($_REQUEST['fluid']) { $fluid = $_REQUEST['fluid']; } else { $fluid = 'beer'; }
	if($_REQUEST['location']) { $location = $_REQUEST['location']; } else { $location = 'wall'; }

	/* CREATE LYRICS */
	for($i = $bottleNumber; $i >= 0; $i--) {

		/* SET DEFAULTS */
		$bottleCount = $i;
		$bottle = 'bottles';
		$action = 'Take one down and pass it around';
		$remainingBottles = $i-1;

		/* HANDLE EXCEPTIONS */
		if($i == 0) {
			$bottleCount = 'no more';
			$action = 'Go to the store and buy some more';
			$remainingBottles = $bottleNumber;
		}
		if($i == 1) {
			$bottle = 'bottle';
			$action = 'Take it down and pass it around';
			$remainingBottles = 'no more';
		}

		/* BUILD LYRICS */
		$lyrics .= ucfirst($bottleCount).' '.$bottle.' of '.$fluid.' on the '.$location.',
'.$bottleCount.' '.$bottle.' of '.$fluid.'.<br>'."\n";
		$lyrics .= $action.', '.$remainingBottles.' bottles of '.$fluid.' on the '.$location.'.<p
\>'."\n\n";
	}

	/* OUTPUT TO PAGE */
	echo $lyrics;
?>
~~~

# R

~~~r
# 99 bottles of beer, alternate R version.
# Demonstrates inherent vectorization of R.
# Creates the correct lyrics, not as in the 
# original BASIC example. 
# Works for any n>2, default n=99. 
# Should work in S-Plus as well.
#
# Giovanni Millo, Trieste (Italy) 4/10/2005
###########################

song<-function(n=99) {
  a<-" bottle"
  b<-" of beer on the wall, "
  c<-" of beer. "
  d<-"Take one down and pass it around: "
  #
  ## set "beer counter" vector
  counter<-c(as.character(n:1),"No more")
  # 
  ## set plural 
  s<-ifelse(counter=="1","","s")
  #
  ## build up the verses vector
  firsthalf<-paste(counter,a,s,b,counter,a,s,c,sep="")
  sechalf1.99<-paste(d,counter[-1],a,s[-1],b,sep="")
  sechalf100<-paste("Go to the store and buy some more: ",
                    counter[1],a,"s",b,sep="")
  ##
  ## song is the vector of n+1 complete verses
  song<-paste(firsthalf,c(sechalf1.99,sechalf100),sep="")
}  

print(song)
~~~

# Ruby

~~~ruby
# 99 Bottles of beer, in Ruby
# By Victor Borja, Sep 14, 2006
# This one shows my favorite Ruby features:
#   continuations, open classes, singleton classes, blocks and being funny!

class Integer # The bottles
  def drink; self - 1; end
end

class << song = nil
  attr_accessor :wall

  def bottles
    (@bottles.zero? ? "no more" : @bottles).to_s <<
      " bottle" << ("s" unless @bottles == 1).to_s
  end
  
  def of(bottles)
    @bottles = bottles
    (class << self; self; end).module_eval do
      define_method(:buy) { bottles }
    end
    self
  end
  
  def sing(&step)
    puts "#{bottles.capitalize} of beer on the wall, #{bottles} of beer."
    if @bottles.zero?
      print "Go to the store buy some more, "
      step = method :buy
    else
      print "Take one down and pass it around, "
    end
    @bottles = step[@bottles]
    puts "#{bottles} of beer on the wall."
    puts "" or wall.call unless step.kind_of? Method
  end

end

callcc { |song.wall| song.of(99) }.sing { |beer| beer.drink }
~~~

# UnrealScript (no syntax highlighting)

~~~
// 99 Bottles of Beer, in UnrealScript. Sings to every player
// simultaneously. Written for UT2003, but would work on other Unreal
// Engine games with little to no modification.
// Probably works on a network. I haven't checked.

// Written by "Dezro" Dave Smith: dezro@mac.com

class MutBottlesBeer extends Mutator;

var int StartingBottles;
var int CurrentBottle;
var int SongLine;
var bool Begun;
var bool StopSong;

function ModifyPlayer(Pawn Other)
{
	Super.ModifyPlayer(Other);
	if (!Begun)
	{
		CurrentBottle = StartingBottles;
		SetTimer(1.5, true);
		Begun = true;
	}
}

function Timer()
{
	local Controller C;
	local bool PassAround;

	if (StopSong)
	{
		SetTimer(0.001, false);
		return;
	}

	for (C = Level.ControllerList; C != None; C = C.NextController)
	{
		Switch (SongLine)
		{
		Case 0:
			if (CurrentBottle == 1)
				C.Pawn.ClientMessage("One bottle of beer on the wall,");
			else
				C.Pawn.ClientMessage(CurrentBottle $ " bottles of beer on the wall,");
			Break;
		Case 1:
			if (CurrentBottle == 1)
				C.Pawn.ClientMessage("One bottle of beer.");
			else
				C.Pawn.ClientMessage(CurrentBottle $ " bottles of beer.");
			Break;
		Case 2:
			if (CurrentBottle == 1)
				C.Pawn.ClientMessage("Take it down, pass it around,");
			else
				C.Pawn.ClientMessage("Take one down, pass it around,");
			PassAround = true;
			Break;
		Default:
			if (CurrentBottle == 1)
				C.Pawn.ClientMessage("One bottle of beer on the wall.");
			else if (CurrentBottle < 1)
			{
				C.Pawn.ClientMessage("No more bottles of beer on the wall!");
				StopSong = true;
			}
			else
				C.Pawn.ClientMessage(CurrentBottle $ " bottles of beer on the wall.");
			Break;
		}
	}
	if (PassAround)
	{
		CurrentBottle--;
		PassAround = false;
	}
	SongLine++;
	if (SongLine > 3)
		SongLine = 0;
}

defaultproperties {
	FriendlyName="99 Bottles"
	Description="Sings to you during the battle."
	StartingBottles = 99
}
~~~
