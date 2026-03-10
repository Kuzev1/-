create database Demo;
use Demo;

create table Customers (
    customer_id int auto_increment primary key,
    ime varchar(100) not null,
    egn varbinary(255)not null,
    phone_number varchar(20)
) engine=InnoDB;

create table Accounts (
    account_id int auto_increment primary key,
    customer_id int not null, 
    iban varchar(22) not null,
    encrypt1 varbinary(255),
    balance decimal(10, 2),
    constraint fk foreign key (customer_id) references Customers(customer_id) on delete cascade 
) engine=InnoDB;

insert into Customers (ime,egn,phone_number)
values ('Иво Кузев', aes_encrypt('0455555555', 'safebank_2026'), '0877112233'),
       ('Иван Иванов', aes_encrypt('044444444', 'safebank_2026'), '0899445566');
       
insert into Accounts (customer_id,iban,encrypt1,balance)
values (1, 'BG88BNBG93001234567801', aes_encrypt('ПИН код за сейф: 1234', 'safebank_2026'), 1500.00),
       (2, 'BG88BNBG93001234567802', aes_encrypt('Въпрос за сигурност: Името на вашата майка - Мария', 'safebank_2026'), 3200.50);
       

       
select ime, egn from Customers;
select iban,encrypt1,balance from Accounts;       
select c.ime, cast(aes_decrypt(c.egn, 'safebank_2026') as char) as EGN,a.iban,
cast(aes_decrypt(a.encrypt1, 'safebank_2026') as char) as Secret_Info,
    a.balance
from Customers c
join Accounts a on c.customer_id = a.customer_id;
