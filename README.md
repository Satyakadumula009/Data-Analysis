# IMDB-SQL-Project
Collected important insights and useful information from the raw data using Python and SQL
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import sqlite3 as sql
# from operator import methodcaller
# 1.connect to database or create a connection --> sql.connect(database_name)
# 2.using the cursor function --> database_variable.cursor()

# workflow:
# 1. establish a connection to sqlite database by creating a connection_object
# 2. create a cursor_object using cursor() method
# 3. execute the query --> cursor_object.execute('query')
# 4. To fetch the data, use fetchall() method of the cursor_object.
db = '/content/drive/MyDrive/Python/movie.sqlite'
con = sql.connect(db)
cur = con.cursor()
cur.execute('Select * from movies')
movies = cur.fetchall()
# To open the 'movies' table
df = pd.DataFrame(movies, columns = ['id', 'original_title', 'budget', 'popularity', 'release_date', 'revenue', 'title', 'vote_average', 'vote_count', 'overview', 'tagline', 'uid', 'director_id'])
df.head(3)
df.shape
df.head(3)
df.info()
# To open the 'directors' table

cur.execute('select * from directors')
director = cur.fetchall()
df = pd.DataFrame(director,columns = ['name','id','gender','uid','department'])
df
# To find the no of movies present in iMDB

cur.execute('select count(*) from movies;')
count = cur.fetchall()
count
# To find directors: James Cameron; Luc Besson; John Woo

cur.execute("select * from directors where name == 'James Cameron' or name == 'Luc Besson' or name == 'John Woo';")
dir3 = cur.fetchall()
dir3 = pd.DataFrame(dir3,columns = ['name','id','gender','uid','profession role'])
dir3
# To find all the directors whose names start with Steven

cur.execute('select * from directors where name like "Steven%";')
steven = cur.fetchall()
steven
cur.execute('select count(*) from directors where gender==1')
gend = cur.fetchall()
gend
cur.execute("SELECT name FROM directors WHERE gender == 1 ORDER by id asc limit 1 OFFSET 9")
name10th = cur.fetchall()
name10th
# To get the most popular movies

cur.execute('SELECT original_title FROM movies ORDER by popularity desc LIMIT 3;')
name = cur.fetchall()
name
# To find the huge budget made films
cur.execute('SELECT original_title FROM movies ORDER by budget desc LIMIT 3')
bankable = cur.fetchall()
bankable
cur.execute('SELECT original_title FROM movies WHERE release_date >="2000-01-01" ORDER by vote_average desc LIMIT 1')
avg_vote = cur.fetchall()
avg_vote
# To find the movies directed by Brenda Chapman

cur.execute('SELECT original_title FROM movies JOIN directors WHERE movies.director_id = directors.id AND directors.name = "Brenda Chapman"')
brenda = cur.fetchall()
brenda
# the director who is most bankable

cur.execute('select name from directors join movies where movies.director_id==directors.id group by directors.id order by sum(budget) desc limit 1')
name = cur.fetchall()
name
# top 10 highest budget movies

cur.execute('select original_title,name,release_date,budget,revenue from movies as mo join directors as dr where mo.director_id==dr.id order by budget desc limit 10')
t10 = cur.fetchall()
t10=pd.DataFrame(t10,columns = ['Movie','Director','Released On','Budget','Revenue'])
t10
# the top 10 films

cur.execute('select original_title,name,release_date,budget,popularity,revenue from movies join directors where movies.director_id == directors.id order by popularity desc limit 10')
pop = cur.fetchall()
pop=pd.DataFrame(pop,columns = ['original_title','name','release_date','budget','popularity','revenue'])
pop
# most popular movies with vote average

cur.execute('select original_title,name,release_date,budget,popularity,vote_average from movies join directors where movies.director_id = directors.id order by popularity desc')
va = cur.fetchall()
va = pd.DataFrame(va,columns = ['original_title','name','release_date','budget','popularity','vote_average'])
va.head(10)
cur.execute('select name,count(name),popularity,revenue from movies join directors where movies.director_id=directors.id group by directors.id order by count(name) desc')
count = cur.fetchall()
count = pd.DataFrame(count,columns = ['name','no.of.movies','popularity','revenue'])
count.head(10)
cur.execute('select original_title,release_date,budget,revenue,popularity,vote_average from movies join directors on movies.director_id=directors.id where name="Steven Spielberg"')
steven = cur.fetchall()
steven = pd.DataFrame(steven,columns = ['Movie','release date','budget','revenue','popularity','vote_avg'])
steven
