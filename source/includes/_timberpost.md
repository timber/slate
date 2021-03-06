
# Timber\Post
This is the object you use to access or extend WordPress posts. Think of it as Timber's (more accessible) version of WP_Post. This is used throughout Timber to represent posts retrieved from WordPress making them available to Twig templates. See the PHP and Twig examples for an example of what it's like to work with this object in your code.

###### PHP
```php
<?php
// single.php, see connected twig example
$context = Timber::get_context();
$context['post'] = new Timber\Post(); // It's a new Timber\Post object, but an existing post from WordPress.
Timber::render('single.twig', $context);
?>
```
###### Twig
```twig
{# single.twig #}
<article>
    <h1 class="headline">{{post.title}}</h1>
    <div class="body">
        {{post.content}}
    </div>
</article>
```
###### HTML
```html
<article>
    <h1 class="headline">The Empire Strikes Back</h1>
    <div class="body">
        It is a dark time for the Rebellion. Although the Death Star has been destroyed, Imperial troops have driven the Rebel forces from their hidden base and pursued them across the galaxy.
    </div>
</article>
```

Name | Type | Description
---- | ---- | -----------
[author](#author) | \Timber\User/null | A User object if found, false if not
[categories](#categories) | array | of TimberTerms
[category](#category) | \Timber\TimberTerm/null | 
[children](#children) | array | 
class | string | $class stores the CSS classes for the post (ex: "post post-type-book post-123")
[comments](#comments) | bool/array | 
[content](#content) | string | 
[date](#date) | string | 
[format](#format) | mixed | 
id | string | $id the numeric WordPress id of a post
[link](#link) | string | ex: http://example.org/2015/07/my-awesome-post
[next](#next) | mixed | 
[parent](#parent) | bool/\Timber\Timber\Post | 
[password_required](#password_required) | boolean | 
[path](#path) | string | 
post_status | string | 		$post_status 	the status of a post ("draft", "publish", etc.)
post_type | string | 	$post_type 		the name of the post type, this is the machine name (so "my_custom_post_type" as opposed to "My Custom Post Type")
[prev](#prev) | mixed | 
slug | string | 	$slug 		the URL-safe slug, this corresponds to the poorly-named "post_name" in the WP database, ex: "hello-world"
[tags](#tags) | array | 
[terms](#terms) | array | 
[thumbnail](#thumbnail) | \Timber\TimberImage/null | of your thumbnail
[time](#time) | string | 
[title](#title) | string | 

## __construct
`__construct( mixed $pid=null )`

**returns:** `void` 

If you send the constructor nothing it will try to figure out the current post id based on being inside The_Loop

Name | Type | Description
---- | ---- | -----------
$pid | mixed | 

###### PHP
```php
<?php
	$post = new Timber\Post();
	$other_post = new Timber\Post($random_post_id);
```

## __toString
`__toString( )`

**returns:** `string` 

Outputs the title of the post if you do something like `<h1>{{post}}</h1>`



## author
`author( )`

**returns:** `\Timber\User/null` A User object if found, false if not

Return the author of a post

###### Twig
```twig
	<h1>{{post.title}}</h1>
	<p class="byline">
	    <a href="{{post.author.link}}">{{post.author.name}}</a>
	</p>
```

## authors
`authors( )`

**returns:** `void` 



## categories
`categories( )`

**returns:** `array` of TimberTerms

Get the categoires on a particular post



## category
`category( )`

**returns:** `\Timber\TimberTerm/null` 

Returns a category attached to a post



## children
`children( string/string/array $post_type="any", bool/string/bool $childPostClass=false )`

**returns:** `array` 

Returns an array of children on the post as Timber\Posts (or other claass as you define).

Name | Type | Description
---- | ---- | -----------
$post_type | string/string/array | _optional_ use to find children of a particular post type (attachment vs. page for example). You might want to restrict to certain types of children in case other stuff gets all mucked in there. You can use 'parent' to use the parent's post type or you can pass an array of post types.
$childPostClass | bool/string/bool | _optional_ a custom post class (ex: 'MyTimber\Post') to return the objects as. By default (false) it will use Timber\Post::$post_class value.

###### Twig
```twig
	{% if post.children %}
	    Here are the child pages:
	    {% for child in post.children %}
	        <a href="{{ child.link }}">{{ child.title }}</a>
	    {% endfor %}
	{% endif %}
```

## comment_form
`comment_form( array $args=array() )`

**returns:** `string` of HTML for the form

Gets the comment form for use on a single article page

Name | Type | Description
---- | ---- | -----------
$args | array | 



## comments
`comments( mixed/int $count=null, string $order="wp", string $type="comment", string $status="approve", string $CommentClass="Timber\Comment" )`

**returns:** `bool/array` 

Gets the comments on a Timber\Post and returns them as an array of [TimberComments](#TimberComment) (or whatever comment class you set).

Name | Type | Description
---- | ---- | -----------
$count | mixed/int | Set the number of comments you want to get. `0` is analogous to "all"
$order | string | use ordering set in WordPress admin, or a different scheme
$type | string | For when other plugins use the comments table for their own special purposes, might be set to 'liveblog' or other depending on what's stored in yr comments table
$status | string | Could be 'pending', etc.
$CommentClass | string | What class to use when returning Comment objects. As you become a Timber pro, you might find yourself extending TimberComment for your site or app (obviously, totally optional)

###### Twig
```twig
	{# single.twig #}
	<h4>Comments:</h4>
	{% for comment in post.comments %}
		<div class="comment-{{comment.ID}} comment-order-{{loop.index}}">
			<p>{{comment.author.name}} said:</p>
			<p>{{comment.content}}</p>
		</div>
	{% endfor %}
```

## content
`content( int $page, mixed $len=-1 )`

**returns:** `string` 

Gets the actual content of a WP Post, as opposed to post_content this will run the hooks/filters attached to the_content. \This guy will return your posts content with WordPress filters run on it (like for shortcodes and wpautop).

Name | Type | Description
---- | ---- | -----------
$page | int | 
$len | mixed | 

###### Twig
```twig
	<div class="article">
	    <h2>{{post.title}}</h2>
	    <div class="content">{{ post.content }}</div>
	</div>
```

## convert
`convert( array/\Timber\WP_Post $data, string $class )`

**returns:** `void` 

Finds any WP_Post objects and converts them to Timber\Posts

Name | Type | Description
---- | ---- | -----------
$data | array/\Timber\WP_Post | 
$class | string | 



## date
`date( string $date_format="" )`

**returns:** `string` 

Get the date to use in your template!

Name | Type | Description
---- | ---- | -----------
$date_format | string | 

###### Twig
```twig
	Published on {{ post.date }} // Uses WP's formatting set in Admin
	OR
	Published on {{ post.date | date('F jS') }} // Jan 12th
```
###### HTML
```html
	Published on January 12, 2015
	OR
	Published on Jan 12th
```

## edit_link
`edit_link( )`

**returns:** `bool/string` the edit URL of a post in the WordPress admin

Returns the edit URL of a post if the user has access to it



## format
`format( )`

**returns:** `mixed` 



## get_comment_count
`get_comment_count( )`

**returns:** `int` the number of comments on a post



## get_content
`get_content( mixed/int $len=-1, int $page )`

**returns:** `string` 

Displays the content of the post with filters, shortcodes and wpautop applied

Name | Type | Description
---- | ---- | -----------
$len | mixed/int | 
$page | int | 

###### Twig
```twig
		<div class="article-text">{{post.get_content}}</div>
```
###### HTML
```html
		<div class="article-text"><p>Blah blah blah</p><p>More blah blah blah.</p></div>
```

## get_field
`get_field( string $field_name )`

**returns:** `mixed` 

Name | Type | Description
---- | ---- | -----------
$field_name | string | 



## get_image
`get_image( string $field )`

**returns:** `\Timber\TimberImage` 

Name | Type | Description
---- | ---- | -----------
$field | string | 



## get_method_values
`get_method_values( )`

**returns:** `array` 



## get_paged_content
`get_paged_content( )`

**returns:** `string` 



## <strike>get_post_type</strike>
**DEPRECATED** since 1.0.4

`get_post_type( )`

**returns:** `\Timber\PostType` 

Returns the post_type object with labels and other info

###### Twig
```twig
	This post is from <span>{{ post.get_post_type.labels.plural }}</span>
```
###### HTML
```html
	This post is from <span>Recipes</span>
```

## <strike>get_preview</strike>
**DEPRECATED** since 1.3.1, use {{ post.preview }} instead

`get_preview( mixed/int $len=50, bool $force=false, string $readmore="Read More", bool/bool/string $strip=true, string $end="&hellip;" )`

**returns:** `string` of the post preview

get a preview of your post, if you have an excerpt it will use that, otherwise it will pull from the post_content. If there's a <!-- more --> tag it will use that to mark where to pull through.

Name | Type | Description
---- | ---- | -----------
$len | mixed/int | The number of words that WP should use to make the tease. (Isn't this better than [this mess](http://wordpress.org/support/topic/changing-the-default-length-of-the_excerpt-1?replies=14)?). If you've set a post_excerpt on a post, we'll use that for the preview text; otherwise the first X words of the post_content
$force | bool | What happens if your custom post excerpt is longer then the length requested? By default (`$force = false`) it will use the full `post_excerpt`. However, you can set this to true to *force* your excerpt to be of the desired length
$readmore | string | The text you want to use on the 'readmore' link
$strip | bool/bool/string | true for default, false for none, string for list of custom attributes
$end | string | The text to end the preview with (defaults to ...)

###### Twig
```twig
	<p>{{post.get_preview(50)}}</p>
```

## has_field
`has_field( string $field_name )`

**returns:** `boolean` 

Name | Type | Description
---- | ---- | -----------
$field_name | string | 



## has_term
`has_term( string/int $term_name_or_id, string $taxonomy="all" )`

**returns:** `bool` 

Name | Type | Description
---- | ---- | -----------
$term_name_or_id | string/int | 
$taxonomy | string | 



## import_field
`import_field( string $field_name )`

**returns:** `void` 

Name | Type | Description
---- | ---- | -----------
$field_name | string | 



## link
`link( )`

**returns:** `string` ex: http://example.org/2015/07/my-awesome-post

get the permalink for a post object

###### Twig
```twig
	<a href="{{post.link}}">Read my post</a>
```

## meta
`meta( mixed/string $field_name=null )`

**returns:** `mixed` 

Name | Type | Description
---- | ---- | -----------
$field_name | mixed/string | 



## modified_author
`modified_author( )`

**returns:** `\Timber\User/null` A User object if found, false if not

Get the author (WordPress user) who last modified the post

###### Twig
```twig
	Last updated by {{ post.modified_author.name }}
```
###### HTML
```html
	Last updated by Harper Lee
```

## modified_date
`modified_date( string $date_format="" )`

**returns:** `string` 

Name | Type | Description
---- | ---- | -----------
$date_format | string | 



## modified_time
`modified_time( string $time_format="" )`

**returns:** `string` 

Name | Type | Description
---- | ---- | -----------
$time_format | string | 



## name
`name( )`

**returns:** `string` 



## next
`next( bool $in_same_term=false )`

**returns:** `mixed` 

Name | Type | Description
---- | ---- | -----------
$in_same_term | bool | 



## paged_content
`paged_content( )`

**returns:** `string` 



## pagination
`pagination( )`

**returns:** `array` 

Get a data array of pagination so you can navigate to the previous/next for a paginated post



## parent
`parent( )`

**returns:** `bool/\Timber\Timber\Post` 

Gets the parent (if one exists) from a post as a Timber\Post object (or whatever is set in Timber\Post::$PostClass)

###### Twig
```twig
	Parent page: <a href="{{ post.parent.link }}">{{ post.parent.title }}</a>
```

## password_required
`password_required( )`

**returns:** `boolean` 

whether post requires password and correct password has been provided



## path
`path( )`

**returns:** `string` 

Gets the relative path of a WP Post, so while link() will return http://example.org/2015/07/my-cool-post this will return just /2015/07/my-cool-post

###### Twig
```twig
	<a href="{{post.path}}">{{post.title}}</a>
```

## <strike>permalink</strike>
**DEPRECATED** 0.20.0 use link() instead

`permalink( )`

**returns:** `string` 



## prev
`prev( bool $in_same_term=false )`

**returns:** `mixed` 

Get the previous post in a set

Name | Type | Description
---- | ---- | -----------
$in_same_term | bool | 

###### Twig
```twig
	<h4>Prior Entry:</h4>
	<h3>{{post.prev.title}}</h3>
	<p>{{post.prev.get_preview(25)}}</p>
```

## preview
`preview( )`

**returns:** `\Timber\PostPreview` 



## tags
`tags( )`

**returns:** `array` 

Gets the tags on a post, uses WP's post_tag taxonomy



## terms
`terms( string/string/array $tax="", bool $merge=true, string $TermClass="" )`

**returns:** `array` 

Get the terms associated with the post This goes across all taxonomies by default

Name | Type | Description
---- | ---- | -----------
$tax | string/string/array | What taxonom(y|ies) to pull from. Defaults to all registered taxonomies for the post type. You can use custom ones, or built-in WordPress taxonomies (category, tag). Timber plays nice and figures out that tag/tags/post_tag are all the same (and categories/category), for custom taxonomies you're on your own.
$merge | bool | Should the resulting array be one big one (true)? Or should it be an array of sub-arrays for each taxonomy (false)?
$TermClass | string | 

###### Twig
```twig
	<section id="job-feed">
	{% for post in job %}
	<div class="job">
	  <h2>{{ post.title }}</h2> 
	   <p>{{ post.terms('category') | join(', ') }}
	 </div>
	{% endfor %}    
	</section>
```
###### HTML
```html
	<section id="job-feed">
	  <div class="job">
		   <h2>Cheese Maker</h2>
	    <p>Food, Cheese, Fromage</p>
	  </div>
	  <div class="job">
		   <h2>Mime</h2>
	    <p>Performance, Silence</p>
	  </div>
	</section>
```

## thumbnail
`thumbnail( )`

**returns:** `\Timber\TimberImage/null` of your thumbnail

get the featured image as a TimberImage

###### Twig
```twig
	<img src="{{post.thumbnail.src}}" />
```

## time
`time( string $time_format="" )`

**returns:** `string` 

Get the time to use in your template

Name | Type | Description
---- | ---- | -----------
$time_format | string | 

###### Twig
```twig
	Published at {{ post.time }} // Uses WP's formatting set in Admin
	OR
	Published at {{ post.time | time('G:i') }} // 13:25
```
###### HTML
```html
	Published at 1:25 pm
	OR
	Published at 13:25
```

## title
`title( )`

**returns:** `string` 

Returns the processed title to be used in templates. This returns the title of the post after WP's filters have run. This is analogous to `the_title()` in standard WP template tags.

###### Twig
```twig
	<h1>{{ post.title }}</h1>
```

## type
`type( )`

**returns:** `\Timber\PostType` 

Returns the post_type object with labels and other info

###### Twig
```twig
	This post is from <span>{{ post.type.labels.name }}</span>
```
###### HTML
```html
	This post is from <span>Recipes</span>
```

## update
`update( string $field, mixed $value )`

**returns:** `void` 

updates the post_meta of the current object with the given value

Name | Type | Description
---- | ---- | -----------
$field | string | 
$value | mixed | 



## get_post_preview_id
`get_post_preview_id( mixed $query )`

**returns:** `mixed` 

Name | Type | Description
---- | ---- | -----------
$query | mixed | 



## get_wp_link_page
`get_wp_link_page( int $i )`

**returns:** `string` 

Name | Type | Description
---- | ---- | -----------
$i | int | 



## maybe_show_password_form
`maybe_show_password_form( )`

**returns:** `string/void` 

If the Password form is to be shown, show it!






*This class extends \Timber\Core*

*This class implements \Timber\CoreInterface*

