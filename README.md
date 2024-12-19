# Source
Source Talents


select distinct
 
Users.DisplayName,
 
Users.Location,
 
Users.CreationDate as RegistrationDate,
 
Users.LastAccessDate,
 
Users.Reputation,
 
Users.Views,
 
concat('https://stackoverflow.com/users/', Users.Id) as ProfileUrl,
 
Users.WebsiteUrl,
 
concat('https://www.google.com/search/?q=', lower(replace(Users.DisplayName collate database_default, ' ', '+'))) as GoogleSearchUrl,
 
concat('https://plus.google.com/s/', lower(replace(Users.DisplayName collate database_default, ' ', '+'))) as GooglePlusSearchUrl,
 
concat('https://github.com/search?type=Users&q=', lower(replace(Users.DisplayName collate database_default, ' ', '+'))) as GitHubSearchUrl,
 
concat('https://www.linkedin.com/vsearch/p?keywords=', lower(replace(Users.DisplayName collate database_default, ' ', '+'))) as LinkedInSearchUrl,
 
concat('https://twitter.com/search?mode=users&q=', lower(replace(Users.DisplayName collate database_default, ' ', '+'))) as TwitterSearchUrl
 
from
 
Users
 
inner join Posts as Answers on Answers.OwnerUserId = Users.Id and
 
Answers.PostTypeId = 2 and
 
Answers.Score >= 1
 
inner join Posts as Questions on Questions.Id = Answers.ParentId
 
inner join PostTags on PostTags.PostId = Questions.Id
 
inner join Tags on Tags.Id = PostTags.TagId
 
where
 
Users.Reputation >= 100 and
 
Users.LastAccessDate >= dateadd(month, -24, getdate()) and
 
(
 
lower(Users.Location) like '%chennai%' or lower(Users.Location) like '%bangalore%' or lower(Users.Location) like '%hyderabad%' or lower(Users.Location) like '%bengaluru%'
 
) and
 
Tags.TagName in (
 
'java'
 
)
 
order by
 
Users.Reputation desc
