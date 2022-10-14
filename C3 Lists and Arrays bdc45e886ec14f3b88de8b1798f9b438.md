# C3 Lists and Arrays

> A *list* is an ordered collection of scalars.
> 

> An *array* is a variable that contains a list.
> 

![A list with five elements](C3%20Lists%20and%20Arrays%20bdc45e886ec14f3b88de8b1798f9b438/Untitled.png)

A list with five elements

Each *element* of an array or list is a separate scalar value. The elements of an array or a list are *indexed* by integers starting at zero and counting by ones.

A list or array may hold numbers, strings, *undef*, or any mixture of different scalar values.

---

## Accessing Elements of an Array

The array elements are numbered using sequential integers, beginning at 0 and increasingly by 1 for each element.

```perl
$fred[0] = "yabba";
$fred[1] = "dabba";
$fred[2] = "doo";
```

Use an array element like $fred[2] in (almost) every place where you could use any other scalar variable. E.g. get the value from an array element or change that value

```perl
print $fred[0];
$fred[2] = "diddley";
$fred[1] .= "whatsis";
```

If the subscript is not a integer, Perl will automatically truncate it (not round) to the whole number portion:

```perl
$number = 2.71828;
print $fred[$number] - 1]; # Same as printing $fred[1]
```

If the subscript indicates an element that would be beyond the end of the array, the corresponding value will be *undef*.

```perl
$blank = $fred[ 142_857]; # unused array element give undef
$blanc = $mel; # Unused scalar $mel also gives undef
```

---

## Special Array Indices

If you assign to an array element that is beyond the end of the array, the array is automatically extended as needed.

```perl
$rocks[0] = 'bedrock'; # One element ...
$rocks[1] = 'slate'; # another ...
$rocks[2] = 'lava'; # and another ...
$rocks[99] = 'schist'; # now there are 95 undef elements
```

The last element index in an array `$#array`.

```perl
$end = $#rocks; # 99， which is the last element's index
$number_of_rocks = $end + 1；# Ok, but you'ill see a better way later
$rocks[$#rocks] = 'hard rock'; # the last rock
```

Use negative array indices count from the end of the array.

```perl
$rocks[-1] = 'hard rock'; # easier way to do that example
$dead_rock = $rocks[-100]; # gets 'bedrock'
$rocks[-200] = 'crystal'; # fatal error!
```

---

## List Literals

A *list* *literal* (the way you represent a list value within your program) is a list of comma-separated values enclosed in parentheses.

```perl
(1, 2, 3) # list of three values 1, 2, and 3
(1, 2, 3,) # the same values (the trailing comma is ignored)
("fred", 4.5) # two values, "fred" and 4.5
( ) # empty list - zero elements
```

Use **..** range operator to create a list of values by counting from the left scalar up to the right scalar by ones.

```perl
(1..100) # list of 100 integers
(1..5) # same as (1, 2, 3, 4, 5)
(1.7..5.7) # same thing; both values are truncataed
(0, 2..6, 10, 12) # same as (0, 2, 3, 4, 5, 6, 10, 12)

# The range operator only counts up, so this won't work and you'll get the empty list:
(5..1) # empty list; ... only counts "uphill"
```

The elements of a list literal are not necessarily constants; they can be expressions that will be newly evaluated each time the literal is used.

```perl
($m, 17) # two values: the current value of $m, and 17
($m+$o, $p+$q) # two values
($m..$n) # range determined by current values of $m and $n
(0..$#rocks) # the indices of the rocks array from the previous section
```

---

## The qw Shortcut

The **qw** shortcut makes it easy to generate a list with simple words without typing a lot of extra quote marks:

```perl
("fred", "barney", "betty", "wilma", "dino")

qw(fred barney betty wilma dino) # same as earlier, but lesss typing
```

> **qw** stands for “quoted words” or “quoted by whitespace”. Perl takes it like a single-quoted string. The whitespace disappear and whatever is left becomes the list of items.
> 

```perl
# Since the whitespace is insignificant, here's another (but unusual) way to write that same list:
qw(fred
	 barney    betty
	wilma dino) # same as before, but pretty strange whitespace

# You can't put comments inside a qw list, but you can format your lists with one element per line, which makes it easy to read as a column:
qw(
	fred
	barney
	betty
	wilma
	dino
)
```

The previous examples used parentheses, but Perl lets you choose any punctuation character as the delimiter:

```perl
qw! fred barney betty wilma dino !
qw/ fred barney betty wilma dino /
qw# fred barney betty wilman dino# # like a comment!

# Sometimes the two delimiters can be different. If the opening delimiter is one of those "left" character, the correspoding "right" character is the proper closing delimiter:
qw( fred barney betty wilma dino )
qw{ fred barney betty wilma dino }
qw[ fred barney betty wilma dino ]
qw< fred barney betty wilma dino >
```

If you need to include the closing delimiter within the string as one of the characters, you should use the backslash:

```perl
qw! yahoo\! google ask msn ! # include yahoo! as an element
```

As in single-quoted strings, two consecutive backslash contribute one single backslash to the item:

```perl
qw( This as a \\ real backslash );
```

It could be useful if you need a list of Unix filenames:

```perl
qw{
	/usr/dict/words
	/home/rootbeer/.ispell_english
}
```

---

## List assignment

Assign list values to variables:

```perl
($fred, $barney, $dino) = ("flinstone", "rubble", undef);

($fred, $barney) = ($barney, $fred); # swap those values
($betty[0], $betty[1]) = ($betty[1], $betty[0]);
```

In a list assignment, extra values are silently ignored - Perl figures that if you wanted those values stored somewhere, you would have told it where to store them. Alternatively, if you have too many variables, the extras get the value undef (or the empty list):

```perl
($fred, $barney) = qw< flinstone rubble slate granite >; # two ignored items
($wilma, $dino) = qw[filestone]; # $dino gets undef
```

You could build up an array of strings with a line of code like this:

```perl
($rocks[0], $rocks[1], $rocks[2], $rocks[3]) = qw/ talc mica feldspar quartz /;
```

Use **@** to refer to an entire array

```perl
@rocks = qw/ bedrock slate lava /;
@tiny = ( ); # the empty list
@giant = 1..1e5; # a list with 100,000 elements
@stuff = (@giant, undef, @giant) # a list with 200,000 elements
$dino = "granite";
@quarry = (@rocks, "crushed rock", @tiny, $dino);
# The last assignment gives @quarry the five-element list (bedrock, slate, lava, crushed rock, granite), since @tiny cntributes zero elements to the list.
```

When you copy any array to another array, it’s still a list assignment. The list are simply stored in arrays.

```perl
@copy = @quarry; # copy a list from one array to another
```

---

### The pop and push Operators

The **pop** operator takes the last element off of an array and returns it:

```perl
@array = 5..9;
$fred = pop(@array); # fred gets 9, @array now has (5, 6, 7, 8)
$barney = pop @array; # barney gets 8, @array now has (5, 6, 7)
pop @array; # @array now has (5, 6). (The 7 is discarded.)
```

The last example use pop in a *void context*, which is merely a fancy way of saying the returns value isn’t going anywhere.

If the array is empty, pop leaves it alone and returns undef.

In Perl, as long as you don’t change the meaning by removing the parentheses, they’re optional.

```perl
push(@array, 0); # @array now has (5, 6, 0)
push @array, 8; # @array now has (5, 6, 0, 8)
push @array, 1..10: # @array now has those 10 new elements
@others = qw/ 9 0 2 1 0 /;
push @array, @others; # @array now has those five new elements (19 total)
```

Note that the first argument to push or the only argument for pop must be an array variable — pushing and popping would not make sense on a literal list.

### The shift and unshift Operators

![Untitled](C3%20Lists%20and%20Arrays%20bdc45e886ec14f3b88de8b1798f9b438/Untitled%201.png)

Analogous to pop, shift returns undef if you give it an empty array variable.

### The splice Operator

The splice operator takes up to four arguments, two of which are optional. The first argument is always the array and the second argument is the position where you want to start.

If you only use those two arguments, Perl removes all of the elements from your starting position to the end and returns them to you:

```perl
@array = qw( pebbles dino fred barney betty );
@removed = splice @array, 2; # remove fred and everything after
                             # @removed is qw(fred barney betty)
                             # @array is qw(pebbles dino)
```

Use a third argument to specify a `length`.

```perl
@array = qw( pebbles dino fred barney betty );
@removed = splice @array, 1, 2; # remove dino, fred
                                # @removed is qw(dino fred)
                                # @array is qw(pebbles barney betty)
```

The fourth argument is a replacement list. 

```perl
@array = qw( pebbles dino fred barney betty );
@removed = splice @array, 1, 2, qw(wilma); # remove dino, fred
                                           # @removed is qw(dino fred)
                                           # @array is qw(pebbles wilma 
                                           #               barney betty
```

Insert elements by remove the length of `0` .

```perl
@array = qw( pebbles dino fred barney betty ); 
@removed = splice @array, 1, 0, qw(wilma); # remove nothing
                                           # @removed is qw()
                                           # @array is qw(pebbles wilma dino
                                           #               fred barney betty)
```

Notice that wilma shows up before dino. Perl inserted the replacement list starting at index 1 and moved everything else over.

---

## Interpolating Arrays into Strings

Array can be interpolated values into a double-quoted string.

```perl
@rocks = qw{ flintstone slate rubble };
print "quartz @rocks limestone\n";
# prints five rocks separated by spaces
# There are no extra spaces added before or after an interpolated array; the space can be added manually.
print "Three rocks are: @rocks.\n"
print "There's nothing in the parens (@empty) here.\n";
```

To type an email address in Perl, you should either escape the @ in a double-quoted string or use a single-quoted string:

```perl
$email = "fred\@bedrock.edu"; # Correct
$email = 'fred@bedrock.edu'; # Another way to do that
```

A single element of an array interpolates into its value just like a scalar variable.

```perl
@fred = qw(hello dolly);
$y = 2;
$x = "This is $fred[1]'s place"; # "This is dolly's place"
$x = "This is $fred[$y-1]'s place"; # same thing
```

Delimit the square bracket to follow a simple scalar variable with a left square bracket so that it isn’t considered part of an array reference.

```perl
@fred = qw(eating rocks is wrong);
$fred = "right"; # we are trying to say "this is right[3]"
print "this is $fred[3]\n"; # prints "wrong" using $fred[3]
print "this is ${fred}[3]\n"; # prints "right" (protected by braces)
print "this is $fred" . "[3]\n"; # right again (different string)
print "this is $fred\[3]\n"; # right again (backslash hides it)
```