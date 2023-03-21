# 在几分钟内创建下一个生产就绪的 GraphQL API。

> 原文：<https://medium.com/javarevisited/create-your-next-production-ready-graphql-api-in-minutes-72da70b9a0ed?source=collection_archive---------2----------------------->

![](img/d411000726c37ffaf3df069fe5f3a862.png)

[附身摄影](https://unsplash.com/@possessedphotography?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍照

如果我告诉您，您可以在几分钟内创建一个使用 Postgres 实现持久性的全功能 GraphQL 服务器，那会怎么样？如果您将来更改了表模式，您的 GraphQL API 会自动检测到这种更改并自动进行自我更改？你会相信我吗？听起来很神奇，对吧？好吧，我向你保证这里没有魔法。我们将使用一些看起来像魔术一样的库。激动吗？好吧，让我们看看我们今天将使用哪些工具。

**波斯特格里斯:我们心爱的大象。**

![](img/3efbca83aa7057208b48409bf75ade45.png)

照片由 [Geran de Klerk](https://unsplash.com/@gerandeklerk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/elephant?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

当我说很多的时候，Postgres 有很多特点，哦，天哪，真的很多。事实上，Postgres 比其他任何开源数据库都有更多的特性。你知道 Postgres 也可以处理你的申请的授权部分吗？不仅如此，你还可以用 Postgres 编写你的业务代码。不相信我？今天，我们将做同样的事情，我们的业务代码将只使用 Postgres。

**海报:巫师。**

![](img/915d993f3a61d561d12c308b950ddd77.png)

Artem Maltsev 在 [Unsplash](https://unsplash.com/s/photos/wizard?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

PostGraphile 将为我们做所有的繁重工作，它将与 Postgres DB 连接，并将我们的数据库作为 graphQL API 公开。在 Postgtaphile 的官方[网站](https://www.graphile.org/postgraphile/)上，他们已经将其定义为:

> PostGraphile 会自动检测表、列、索引、关系、视图、类型、函数、注释等——提供一个对数据高度智能的 GraphQL 服务器，当您修改数据库时，它会自动更新自己而无需重新启动。

酷吧？PostGraphile 是一个开源项目，他们创建了如此高效的库，值得称赞。如果你打算在生产中使用 postgrapile，你也可以查看他们的 pro 计划，解锁 postgrapile 的一些新特性。

你可以找到设置 Postgraphile 的说明，而 [Postgres](https://javarevisited.blogspot.com/2020/02/top-5-courses-to-learn-postgresql-in.html) 可以在[这里](https://www.graphile.org/postgraphile/quick-start-guide/)找到。

首先，让我们创建两个模式。

```
create schema medium_example_public;
create schema medium_example_private;
commit;
```

您可以创建任意多的模式，但是在本教程中，我将使用 2 个模式。我将在公共模式和私有模式中创建所有表，其中将有与身份验证相关的表。如果您也打算在生产中使用 Postgraphile，我建议在不同的模式中定义表结构，然后在带有视图的公共模式中公开这些表。

现在，让我们在公共模式中创建一个表来包含作者的数据。

```
create table medium_example_public.author (
 id               serial primary key,
 name       text not null check (char_length(name) < 80),
 about            text,
 created_at       timestamp default now()
); 
```

现在每个作家都可以写关于某个主题的文章，所以让我们首先创建一个具有不同主题名称的 [enum](https://www.java67.com/2018/07/java-enum-with-constructor-example.html) ，稍后在帖子的表格中使用。

```
create type medium_example_public.post_topic as enum (
'Programming',
'Gaming',
'Music',
'Culture'
);
```

现在，让我们再创建一个表来存储所有的文章。

```
create table medium_example_public.post (
id               serial primary key,
author_id        integer not null references
medium_example_public.author(id),
title         text not null check (char_length(title) < 280),
body             text,
topic            medium_example_public.post_topic,
created_at       timestamp default now()
);
```

到目前为止，我们已经创建了 2 个表，我们还可以在自动生成的文档中添加表和列的注释。所以让我们也这样做吧

```
comment on table medium_example_public.author is 'A user of the medium.';
comment on column medium_example_public.author.id is 'The primary unique identifier for the person.';
comment on column medium_example_public.author.name is 'The person’s  name.';
comment on column medium_example_public.author.about is 'A short description about the user, written by the user.';
comment on column medium_example_public.author.created_at is 'The time this person was created.';comment on table medium_example_public.post is 'A forum post written by a author.';
comment on column medium_example_public.post.id is 'The primary key for the post.';
comment on column medium_example_public.post.title is 'The title written by the user.';
comment on column medium_example_public.post.author_id is 'The id of the author.';
comment on column medium_example_public.post.topic is 'The topic this has been posted in.';
comment on column medium_example_public.post.body is 'The main body text of our post.';
comment on column medium_example_public.post.created_at is 'The time this post was created.';
```

现在你已经拥有了运行 GraphQL 的 API 的一切。让我们开始 PostGraphile

```
postgraphile -c postgres://postgres:123456@127.0.0.1/medium --schema medium_example_public
```

我们已经可以运行一些 GraphQL 的查询，让我们假设对于作者的主页，我们需要获取作者的详细信息和作者写的所有帖子。这很容易做到，如下所示。

[![](img/aad7043268c37810da41ff1aa01ca568.png)](https://dev.to/javinpaul/5-best-courses-to-learn-graphql-in-2020-2fo5)

令人难以置信的是，我们已经有一些基本的 GraphQL 查询在运行，对吗？如果你想根据你的业务支持一些其他的 GraphQL，那也可以毫不费力地实现。假设您想在文章的正文和标题中搜索一个字符串。我们可以在 Postgres 中编写函数来支持业务操作。例如，我们可以编写下面的函数来搜索文章。

```
create function medium_example_public.search_posts(search text)
returns setof medium_example_public.post as $$
select post.*
from medium_example_public.post as post
where position(search in post.title) > 0 or position(search in post.body) > 0
$$ language sql stable;comment on function medium_example_public.search_posts(text) is 'Returns posts containing a given search term.';
```

你可以看到它是多么简单。现在我们也来测试一下。

![](img/0a44cba125df993506a76ec1e8008002.png)

你能在这么短的时间内做这么多事情，这是多么酷啊？不仅如此，如果您愿意，还可以在查询中使用分页。

您可能想知道这可能是开发 GraphQL API 最快的方法，但是安全性在哪里呢？嗯，你说得对，没有安全性，我们无法部署任何应用程序。

为了 API 的安全性，我们将使用 [JWT 令牌](/javarevisited/top-10-courses-to-learn-spring-security-and-oauth2-with-spring-boot-for-java-developers-8f0222d6066d)。您需要定义 PostGraphile 将用来生成 JWT 令牌的数据类型。我们需要我们的 JWT 令牌来包含关于作者的帐户 id、角色和令牌到期日期的信息。所以我们将在 Postgres 中创建一个类型，如下所示。

```
create type medium_example_public.jwt_token as (
  role text,
  account_id integer,
  exp bigint
);
```

PostGraphile 将采用这种类型，为我们制作一个 JWT 令牌。我们还需要存储用户的电子邮件和密码，这样一旦用户通过身份验证，我们就可以在输出中给他发送一个 JWT 令牌。让我们在私有模式中为它创建一个表。

```
create table medium_example_private.account (
account_id        integer primary key references medium_example_public.author(id) on delete cascade,
email            text not null unique check (email ~* '^.+@.+\..+$'),
password    text not null
);
comment on table medium_example_private.account is 'login details about authors account.';
comment on column medium_example_private.account.account_id is 'Author Id';
comment on column medium_example_private.account.email is 'The email address of the author.';
comment on column medium_example_private.account.password is 'password of author, stored as hash.'
```

在 account 表中，我们存储了电子邮件 id、散列加密密码和作者 id。现在，让我们创建一个函数，它接受电子邮件和密码作为输入，并在输出中返回 JWT 令牌。

```
create function medium_example_public.authenticate(email text,
password text) returns medium_example_public.jwt_token as $$
declare
account medium_example_private.account;
begin
select a.* into account from medium_example_private.account as a
where a.email = $1;
if account.password = crypt(password, account.password) then
return ('medium_example_author', account.account_id, extract(epoch from (now() + interval '1 days')))::medium_example_public.jwt_token;
else
return null;
end if;
end;
$$ language plpgsql strict security definer;comment on function medium_example_public.authenticate(text, text) is 'Create a JWT token with 1 day of validity';
```

如果你知道基本的 [SQL](/javarevisited/7-free-courses-to-learn-database-and-sql-for-programmers-and-data-scientist-e7ae19514ed2) ，你就已经知道我们在上面的函数中做了什么。我们正在尝试验证用户，如果电子邮件和密码是正确的，我们发送 JWT，否则返回 null。如果你想知道什么是地穴？这是 Postgres 中的一个函数，它将返回字符串的散列值。

看这么多 SQL 烦不烦？再忍耐我两秒钟。让我们快速地写一个函数来注册一个用户。这个函数将在 account 表和 author 表中插入数据。

```
create function medium_example_public.register_author(
name text,
email text,
password text
) returns medium_example_public.author as $$
declare
author medium_example_public.author;
begin
insert into medium_example_public.author (name) values
(name)
returning * into author ;
insert into medium_example_private.account (account_id, email, password) values
(author.id, email, crypt(password, gen_salt('bf')));
return author;
end;
$$ language plpgsql strict security definer;
comment on function medium_example_public.register_author(text, text, text, text) is 'Register an author';
```

现在，我们有了注册用户的方法，登录用户的方法，但是嘿！我想我们还是遗漏了一些东西。我知道，我们缺少角色，每个用户都应该有不同的角色，就像用户应该能够在没有帐户的情况下查看和搜索帖子，每个用户只能访问其个人数据。为此，我们将使用 Postgres 的角色，让我们创建 2 个角色:

```
create role medium_example_anonymous;
create role medium_example_author;
```

我们现在有一个匿名角色和一个作者角色。现在，让我们根据我们的业务在这些表格上申请一些资助。我们的匿名用户应该只能查看帖子，搜索帖子，注册为作者，并查看其他作者。一旦用户登录，他可以修改文章，创建新文章，甚至删除文章。

```
alter default privileges revoke execute on functions from public;
grant usage on schema medium_example_public to medium_example_anonymous, medium_example_author;
grant select on table medium_example_public.author to medium_example_anonymous, medium_example_author;
grant update, delete on table medium_example_public.author to medium_example_author;grant select on table medium_example_public.post to medium_example_anonymous, medium_example_author;
grant insert, update, delete on table medium_example_public.post to medium_example_author;
grant usage on sequence medium_example_public.post_id_seq to medium_example_author;
grant execute on function medium_example_public.search_posts(text) to medium_example_anonymous, medium_example_author;
grant execute on function medium_example_public.authenticate(text, text) to medium_example_anonymous, medium_example_author;
grant execute on function medium_example_public.register_author(text, text, text) to medium_example_anonymous;
```

默认情况下，Postgres 中的函数可以从公众中执行，在上面的第一行中，我们限制了由公众执行的函数。然后我们根据我们的需求授予表和函数。现在让我们试着注册一个用户。

[![](img/d42a8f549ba1ba285f79502ffd975dee.png)](https://javarevisited.blogspot.com/2020/02/top-5-postman-tutorials-and-courses-for-web-developers.html)

很好，我们的用户已经注册成功。现在让我们尝试用这个用户获得一个 JWT 令牌。

![](img/b5fa1a1e2e94808732ff8b4e90e14ab0.png)

正如你所看到的，我们也成功地获得了 JWT 令牌。现在让我们用这个令牌创建一个帖子。

![](img/2f7e27ed97fd8e9563e60bf90c46ba25.png)

现在有一个问题，我们只是验证一个用户是否是我们网站的注册用户，这意味着一个用户也可以代表其他用户发帖。这是不好的，我们肯定要改变它。Postgres 还支持行级锁。因此，我们将使用行级锁，并确保我们的用户只能为自己发帖。

```
alter table medium_example_public.author enable row level security;
alter table medium_example_public.post enable row level security;create policy select_author on medium_example_public.author for select using (true);
create policy select_post on medium_example_public.post for select
using (true);
create policy update_person on medium_example_public.author for update to forum_example_person using (id = nullif(current_setting('jwt.claims.account_id', true), '')::integer);
create policy delete_person on medium_example_public.author for delete to forum_example_person using (id = nullif(current_setting('jwt.claims.account_id', true), '')::integer);
create policy insert_post on medium_example_public.author for insert to forum_example_person with check (author_id = nullif(current_setting('jwt.claims.account_id', true), '')::integer);
create policy update_post on medium_example_public.author for update to forum_example_person using (author_id = nullif(current_setting('jwt.claims.account_id', true), '')::integer);
create policy delete_post on medium_example_public.post for delete to forum_example_person using (author_id = nullif(current_setting('jwt.claims.person_id', true), '')::integer);
```

现在让我们试着用其他用户的作者 id 创建一篇文章

![](img/d4e2f1deb03d1e92dcab493e20579d06.png)

如你所见，我们不允许这样做。好了，今天就到这里吧。如果你读到这里，我真的要感谢你。

# **结论**

当你要开始创业时，尽快实现你的想法是非常重要的。在这种情况下，像 Postgraphile 和 Postgres 这样的工具会派上用场。在开发的大部分时间里，我们浪费时间重新创建已经存在的特性，比如 Postgres 中的授权。想象一下，如果你不得不在 [Spring boot](/javarevisited/top-10-courses-to-learn-spring-boot-in-2020-best-of-lot-6ffce88a1b6e) 中做同样的事情，你将会为认证、基于角色的访问写很多代码，而这些你实际上并不需要。我强烈建议你应该尝试一下 PostGraphile。

# 资源

 [## PostgreSQL 模式设计

### Postgres 数据库的功能非常丰富，远远超过其他任何数据库。然而，大多数开发人员并不…

www.graphile.org](https://www.graphile.org/postgraphile/postgresql-schema-design/) [](https://netflixtechblog.com/beyond-rest-1b76f7c20ef6) [## 超越休息

### GraphQL 微服务的快速开发

netflixtechblog.com](https://netflixtechblog.com/beyond-rest-1b76f7c20ef6)