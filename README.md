# Working-with-KSA-Cinema-Data-Set-inMySQL-from-scratch

# We need to turn the empty rows to null
```
set SQL_SAFE_UPDATES = 0;
update cleaned_file 
set 
    name = NULLIF(TRIM(name), ''),
    review_count = NULLIF(TRIM(review_count), ''),
    genre = NULLIF(TRIM(genre), ''),
    location = NULLIF(TRIM(location), ''),
    best_comment = NULLIF(TRIM(best_comment), '');
set SQL_SAFE_UPDATES = 1;
```

# 1.Get the top 5 highest-rated Cinema.
```
select name, rating
from cleaned_file
where name is not null
order by rating desc
limit 5;
```
<img width="240" alt="image" src="https://github.com/user-attachments/assets/1d00110a-f49a-493b-b97f-0c31eaaed001" />


# 2.Count the number of Cinema by genre.
```
select genre, count(*) count
from cleaned_file
group by genre
order by count desc;
```
<img width="254" alt="image" src="https://github.com/user-attachments/assets/3380c56d-032e-412f-b15f-f7e423e154ec" />

# 3.Find Cinema with missing best comments.
```
select name,best_comment
from cleaned_file 
where best_comment is NULL;
```
<img width="262" alt="image" src="https://github.com/user-attachments/assets/e2404042-0460-4cfb-b98d-729bc397aafd" />


# 4.List all Cinema in a specific city ("Dammam").
```
select name, location 
from cleaned_file
where location like '%Dammam%';
```
<img width="423" alt="image" src="https://github.com/user-attachments/assets/7532a98c-3699-45a4-b19f-eb05e62fa490" />

# 5.Find the average rating for each genre.
```
select genre, avg(rating) avg_rating
from cleaned_file
group by genre
order by avg_rating desc;
```
<img width="385" alt="image" src="https://github.com/user-attachments/assets/1ceeea31-7752-4226-a6b6-d7377e73d1a0" />

# 6.Retrieve Cinema that have the word ‘Theater’ in their name.
```
select name
from cleaned_file
where name like '%Theater%';
```
<img width="184" alt="image" src="https://github.com/user-attachments/assets/57ce0b11-68b0-49bc-82b8-b2ac4f88a097" />

# 7.Count how many Cinema have a rating of 5.
```
select count(*) Five_Star_Cinemas
from cleaned_file
where rating = 5;
```
<img width="121" alt="image" src="https://github.com/user-attachments/assets/0363e235-157b-4982-b9a1-9ef6b56c2bbc" />

# 8.Find Cinema with the longest best comment.
```
select name, length(best_comment) comment_length
from cleaned_file
where best_comment is not null
order by comment_length desc
limit 1;
```
<img width="276" alt="image" src="https://github.com/user-attachments/assets/20713a37-64c4-47df-91a0-9aaee0c8bc57" />

# 9.Get the highest-rated Cinema in each city.
```
select name, location, rating Max_rating
from cleaned_file
where rating = (select max(rating) from cleaned_file);
```
<img width="493" alt="image" src="https://github.com/user-attachments/assets/7980081f-a746-47aa-93b4-d1fa1465651a" />

# 10. Find the Cinema with the most reviews ( Replaceing 000 to K )
```
select name, review_count 
from cleaned_file 
where name is not null
order by cast(replace(review_count, 'K', '000') as unsigned) desc;
```
<img width="265" alt="image" src="https://github.com/user-attachments/assets/ffd1fb21-d499-46bd-bc64-8347ffc88399" />

# Create a Function that returns Dammam's Data when selecting
- Function Code
```
delimiter //
create function get_location()
returns varchar(255)
deterministic
begin
    return 'Dammam\'s Data';
end //
delimiter ;
```
```
select name, rating, review_count, genre, get_location() location , best_comment
from cleaned_file
where best_comment is not null;
```
![Uploading image.png…]()

