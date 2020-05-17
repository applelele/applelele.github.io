---
layout: post
title:      "Cheat sheet for Object Oriented Ruby Relationship"
date:       2020-05-17 05:21:50 +0000
permalink:  cheat_sheet_for_object_oriented_ruby_relationship
---


**Belongs to**
- An object can own another object
- Use an attribute and assign it to equal the object that it belongs to

**song**
belongs to artist

```
class Song
  attr_accessor :title, :artist
	
	def initialize(title)
	  @title = title
  end

end
```

```
drake = Artist.new("Drake", "Hip Hop")
hotline_bling = Song.new("Hotline Bling")
hotline_bling.artist = drake
```

```
hotline_bling
# => something like the following
# => #<Song:000 @title="Hotline Bring" @artist=#<Artist:000 @name="Drake", @genre="Hip Hop">>
```


**Has many**
- Allows one object to own multiple of another object
- Use a plural attribute and set it to equal am empty array

**artist**
have many songs 

```
class Artist
  attr_accessor :name
	
	def initialize(name)
	  @name = name
		@songs = []
	end
	
	def add_song(song)
	  @songs << song
	end

end
```

```
drake = Artist.new("Drake", "Hip Hop")
hotline_bling = Song.new("Hotline Bling")
drake.add_song(hotline_bling)

drake
# => #<Artist:000 @name="Drake", @songs=[#<Song:000 @title="Hotline Bling", @artist=#<Artist:000@name="Drake"...>]>
```

**Has many through**
- Allows an object to have access to many other objects indirectly through another object
- Set up the Artist has many songs relationship
- Set up the Song belongs to a Genre relationship
- The Artist now has many genres through songs


```
class Artist
  attr_accessor :name, :song, :genre
	
	def initialize(name)
	  @name = name
		@songs = [] # <= has many songs
		@genre = genre
	end
	
	def add_song(song)
	  @songs << song
		song.artist = self  
		# tell a song that belongs to an artist should happen when that song is added to the artist's @songs collection
	end
	
	def songs
	  @songs
		# make songs a readable attribute
	end
	
	def genres
	  self.songs.collect do |song|
		  song.genre
		end
		# give the Artist object access to its Genre through its songs
	end

end
```

Refactored the above code following the real world (A song doesn't exist before an artist creates it).
```
class Artist
  attr_accessor :name, :song, :genre
	
	def initialize(name)
	  @name = name
  end
	
	def add_song(song)
	  song.artist = self
	end
	
	def songs
	  Song.all.select { |song| song.artist == self }
	end
	
	def add_song_by_name(name, genre)
	  song = Song.new(name, genre)
		add_song(song)
	end
	
end
```

