# Elindex - full text search in Elixir

I modified the program from the August 2016 meeting in a few ways.
First, I have it load the files in parallel using Task.async similar
to how it was done in the July 2016 meeting. The load files returns
a tuple with the filename and a map of all the counts from that file.
Then I just create the word map by reducing the list of {filename,map}
tuples into the word map, but each one of those requires its word map
to be reduced into the current word map, so there are nested reduces.
On my 8 CPU machine it takes about a minute to load all the files and
generate the map.

I also modified the search so it sorts by most frequent and then returns
the top 10.

This is the program we wrote collaboratively at the August 2016 meeting.
@tattdcodemonkey, @joshcrews, @digitalBush, and @jorendorff worked on it,
among others.

To try it out:

<pre>$ <strong>iex -S mix</strong>
Erlang/OTP 19 [erts-8.0] [source] [64-bit] [smp:8:8] [async-threads:10] [hipe] [kernel-poll:false]

Compiling 2 files (.ex)
Generated elindex app
Interactive Elixir (1.3.2) - press Ctrl+C to exit (type h() ENTER for help)
iex(1)&gt; <strong>Elindex.Searcher.load_files()</strong>
{:ok, #PID<0.148.0>}
iex(2)&gt; <strong>Elindex.Searcher.search("launch")</strong>
[{"1971", 4}, {"2012–13 NHL season", 2}, {"Antrim GAA", 1},
 {"Big Ten Network", 8}, {"Boris Becker", 1}, {"Bryan Adams", 1},
 {"ESPN.com", 2}, {"Huntsville, Alabama", 1}, {"IGN", 3}, {"Johor Bahru", 1}]
</pre>

(To exit `iex`, hit Ctrl-C twice.)
