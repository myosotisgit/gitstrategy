# Naming Conventions voor Developing

Naming conventions voor Laravel en Vue gebaseerd op PHP-FIG (PSR https://www.php-fig.org/psr/).


## Refs
- https://laravel.com/docs/8.x/contributions#coding-style
- https://www.mindtwo.de/guidelines/coding/laravel
- https://webdevetc.com/blog/laravel-naming-conventions/
- https://www.laravelbestpractices.com/

## Samenvatting veel gebruikte termen

What |	How |	Good	| Bad
----|----|----|----
Controller	|singular|	ArticleController|	ArticlesController
Route	|plural|	articles/1|	article/1
Named route	|snake_case with dot notation|	users.show_active	|users.show-active, show-active-users
Model	|singular|	User	|Users
hasOne or belongsTo relationship|	singular|	articleComment	|articleComments, article_comment
All other relationships	|plural|	articleComments	|articleComment, article_comments
Table|	plural|	article_comments	|article_comment, articleComments
Pivot table|	singular model names in alphabetical order|	article_user	|user_article, articles_users
Table column|	snake_case without model name	|meta_title|	MetaTitle; article_meta_title
Foreign key	|singular model name with _id suffix|	article_id|	ArticleId, id_article, articles_id
Primary key|	-|	id|	custom_id
Migration|	-|	2017_01_01_000000_create_articles_table|	2017_01_01_000000_articles
Method	|camelCase	|getAll	|get_all
Function	|snake_case	|abort_if	|abortIf
Method in resource controller|see 'table' |	store	|saveArticle
Method in test class	|camelCase|	testGuestCannotSeeArticle	|test_guest_cannot_see_article
Model property	|snake_case	|$model->model_property	|$model->modelProperty
Variable|	camelCase	|$anyOtherVariable|	$any_other_variable
Collection	|descriptive, plural	|$activeUsers = User::active()->get()	|$active, $data
Object|	descriptive, singular	|$activeUser = User::active()->first()	|$users, $obj
Config and language files index	|snake_case|	articles_enabled	|ArticlesEnabled; articles-enabled
View	|kebab-case	|show-filtered.blade.php	|showFiltered.blade.php, show_filtered.blade.php
Config|	kebab-case	|google-calendar.php	|googleCalendar.php, google_calendar.php
Contract (interface)	|adjective or noun	|Authenticatable	|AuthenticationInterface, IAuthentication
Trait	|adjective	|Notifiable	|NotificationTrait
