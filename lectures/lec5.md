Python UDF	
	
	create function googleit(word text) returns text as $$
	
	import requests
	r = requests.get("http://www.google.com/search?q=%s" % (word,))
	return r.content.decode('unicode-escape')
	
	$$ language plpython2u;
	
	
	
	select left(googleit('columbia university'), 50);
	select googleit('columbia university') LIKE '%Columbia%';
	


copy record

	
	create function copyit() returns trigger as $$
	begin 
		insert into b values(NEW.a); 
		return NEW; 
	end;
	$$ language plpgsql;
	
	create trigger t_copyit after insert on a 
	for each row	 
		when (NEW.a > 10) 
		execute procedure copyit();

Disable inserts to a	
	
 
	
	create trigger t_disable before insert on a 
	for each row 
		execute procedure disable();
	
	
	CREATE FUNCTION checktotal() RETURNS trigger

	begin
		insert into a values(NEW.a);
		return NEW;
	end;
	$$ language plpgsql;
	
	create trigger t_recursive before insert on a
	for each row
		execute procedure addme();
		
	-- also recursive
	create trigger t_recursive after insert on a
	for each row
		execute procedure addme();		
		
Still inorrect trigger

	CREATE FUNCTION addme_stillwrong() RETURNS trigger





	begin
	if ((select count(*) from a) + (select count(*) from b) < 20) then
	  return NEW;
	else
	 return null;
	end if;
	end;
	$$ language plpgsql;
	
	create trigger t_check before insert on a   for each row execute procedure checktotal();		
WITH clause
	
	with num(n) as (
		select count(*) from a
	), 
		 num2(n) as (
		 select count(*) from a
	) 

	select num.n+num2.n from num, num2;	
	
Views

	create view acopy as 
		select * from a;
	insert into acopy values (99);
	
	create view aacopy as 
		select a1.a as a1, a2.a as a2 
		from a as a1, a as a2 
		where a1.a = a2.a;
	insert into aacopy values (88, 99);
	
	create view adistinct as select distinct * from a;
	insert into adistinct values (9);
	
Create table and default column names

	Create table b as select 1;
		