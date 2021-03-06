# Laravel 5* Scaffold Generator

Laravel5* Scaffold All (Model, Controller, views) Generator


## Usage

### Step 1: Install Through Composer

```
composer require scaffolds/builder:"dev-master"
```

### Step 2: Add the Service Provider


Open `config/app.php` and, add to `Scaffold\Builder\GeneratorsServiceProvider::class`

```
'providers' => [
     . . .
     Scaffold\Builder\GeneratorsServiceProvider::class,
],
```


### Step 3: Run Artisan!

~~~
php artisan make:scaffold
~~~



## Examples


```

C:\xampp\htdocs\laravel53>php artisan make:scaffold

 Generate all controller, model and views? [y|N] (yes/no) [no]:
 > y

==================================================

 Notice: Are you backed up if exists files? [y|N] (yes/no) [no]:
 > n

C:\xampp\htdocs\laravel53\resources\views/blogcomments\create.blade.php
C:\xampp\htdocs\laravel53\resources\views/blogcomments\edit.blade.php
C:\xampp\htdocs\laravel53\resources\views/blogcomments\show.blade.php
C:\xampp\htdocs\laravel53\resources\views/blogcomments\index.blade.php
C:\xampp\htdocs\laravel53\resources\views/blogcomments\_form.blade.php
C:\xampp\htdocs\laravel53\app\Http\Controllers\BlogCommentsController.php
C:\xampp\htdocs\laravel53\app\BlogComments.php
C:\xampp\htdocs\laravel53\resources\views/blogposts\create.blade.php
C:\xampp\htdocs\laravel53\resources\views/blogposts\edit.blade.php
C:\xampp\htdocs\laravel53\resources\views/blogposts\show.blade.php
C:\xampp\htdocs\laravel53\resources\views/blogposts\index.blade.php
C:\xampp\htdocs\laravel53\resources\views/blogposts\_form.blade.php
C:\xampp\htdocs\laravel53\app\Http\Controllers\BlogPostsController.php
C:\xampp\htdocs\laravel53\app\BlogPosts.php
C:\xampp\htdocs\laravel53\resources\views/blogtags\create.blade.php
C:\xampp\htdocs\laravel53\resources\views/blogtags\edit.blade.php
C:\xampp\htdocs\laravel53\resources\views/blogtags\show.blade.php
C:\xampp\htdocs\laravel53\resources\views/blogtags\index.blade.php
C:\xampp\htdocs\laravel53\resources\views/blogtags\_form.blade.php
C:\xampp\htdocs\laravel53\app\Http\Controllers\BlogTagsController.php
C:\xampp\htdocs\laravel53\app\BlogTags.php
C:\xampp\htdocs\laravel53\resources\views/blogcategories\create.blade.php
C:\xampp\htdocs\laravel53\resources\views/blogcategories\edit.blade.php
C:\xampp\htdocs\laravel53\resources\views/blogcategories\show.blade.php
C:\xampp\htdocs\laravel53\resources\views/blogcategories\index.blade.php
C:\xampp\htdocs\laravel53\resources\views/blogcategories\_form.blade.php
C:\xampp\htdocs\laravel53\app\Http\Controllers\BlogCategoriesController.php
C:\xampp\htdocs\laravel53\app\BlogCategories.php
C:\xampp\htdocs\laravel53\resources\views/users\create.blade.php
C:\xampp\htdocs\laravel53\resources\views/users\edit.blade.php
C:\xampp\htdocs\laravel53\resources\views/users\show.blade.php
C:\xampp\htdocs\laravel53\resources\views/users\index.blade.php
C:\xampp\htdocs\laravel53\resources\views/users\_form.blade.php
C:\xampp\htdocs\laravel53\app\Http\Controllers\UsersController.php
C:\xampp\htdocs\laravel53\app\Users.php


---------------------------------------------------------------------------

| Route file created: C:\xampp\htdocs\laravel53\routes\scaffold_routes.php
| Please open C:\xampp\htdocs\laravel53\routes\web.php routes file
| add require('scaffold_routes.php');
| and run http://localhost/laravel53/public/yourRoute
| Created crud help code. Please open: C:\xampp\htdocs\laravel53\resources\crud_code_help.txt

---------------------------------------------------------------------------


C:\xampp\htdocs\laravel53>
```



## Scaffold
![image](http://i66.tinypic.com/qqr18g.png)
![image](http://i66.tinypic.com/ic74v8.png)
![image](http://i65.tinypic.com/29lh303.png)


## Example Model

```php
<?php

namespace App;
use Illuminate\Database\Eloquent\Model;

class BlogPosts extends Model {
	
    protected $table = 'blog_posts';
	
    public $timestamps = false;
	
	protected $fillable = ['category_id', 'title', 'summary', 'content', 'slug', 'status', 'comments'];

    public function BlogTags() {
		return $this->belongsToMany('App\BlogTags', 'blog_post_tag', 'post_id', 'tag_id');
    }

    public function BlogCategories() {
		return $this->belongsTo('App\BlogCategories', 'category_id', 'id');
    }

    public function BlogComments() {
		return $this->hasMany('App\BlogComments', 'post_id', 'id');
    }
}
```

## Example Controller

```php
<?php

namespace App\Http\Controllers;

use App\Http\Controllers\Controller;
use Illuminate\Http\Request;

use App\BlogPosts;


class BlogPostsController extends Controller {

	/**
	 * Display a listing of the resource.
	 *
	 * @return Response
	 *
	 * Route::get('blogposts', 'BlogPostsController@index')->name('blogposts.index');
	 */
	public function index()
	{
		$blogposts = BlogPosts::all();

		return view('blogposts.index', compact('blogposts',$blogposts));
	}

	/**
	 * Show the form for creating a new resource.
	 *
	 * @return Response
	 *
	 * Route::get('blogposts/create', 'BlogPostsController@create')->name('blogposts.create');
	 */
	public function create()
	{
		return view('blogposts.create');
	}

	/**
	 * Store a newly created resource in storage.
	 *
	 * @param Request $request
	 * @return Response
	 *
	 * Route::post('blogposts/store', 'BlogPostsController@store');
	 */
	public function store(Request $request)
	{
	     $this->validate($request, [

            'category_id' => 'required|numeric',
            'title' => 'required|max:255',
            'summary' => 'required',
            'content' => 'required',
            'slug' => 'required|max:255',
            'status' => 'required',
            'comments' => 'required|numeric',

		 ]);
		 
		$blogposts = new BlogPosts();

		$blogposts->category_id = $request->input('category_id');
		$blogposts->title = $request->input('title');
		$blogposts->summary = $request->input('summary');
		$blogposts->content = $request->input('content');
		$blogposts->slug = $request->input('slug');
		$blogposts->status = $request->input('status');
		$blogposts->comments = $request->input('comments');

		$blogposts->save();

		return redirect()->route('blogposts.index')->with('message', 'Item created successfully.');
	}

	/**
	 * Display the specified resource.
	 *
	 * @param  int  $id
	 * @return Response
	 *
	 * Route::get('blogposts/show/{id}', 'BlogPostsController@show');
	 */
	public function show($id)
	{
		$blogposts = BlogPosts::findOrFail($id);

		return view('blogposts.show', compact('blogposts',$blogposts));
	}

	/**
	 * Show the form for editing the specified resource.
	 *
	 * @param  int  $id
	 * @return Response
	 *
	 * Route::get('blogposts/edit/{id}', 'BlogPostsController@edit');
	 */
	public function edit($id)
	{
		$blogposts = BlogPosts::findOrFail($id);

		return view('blogposts.edit', compact('blogposts',$blogposts));
	}

	/**
	 * Update the specified resource in storage.
	 *
	 * @param  int  $id
	 * @param Request $request
	 * @return Response
	 *
	 * Route::put('blogposts/update/{id}', 'BlogPostsController@update');
	 */
	public function update(Request $request, $id)
	{
		$blogposts = BlogPosts::findOrFail($id);

		$blogposts->category_id = $request->input('category_id');
		$blogposts->title = $request->input('title');
		$blogposts->summary = $request->input('summary');
		$blogposts->content = $request->input('content');
		$blogposts->slug = $request->input('slug');
		$blogposts->status = $request->input('status');
		$blogposts->comments = $request->input('comments');

		$blogposts->save();

		return redirect()->route('blogposts.index')->with('message', 'Item updated successfully.');
	}

	/**
	 * Remove the specified resource from storage.
	 *
	 * @param  int  $id
	 * @return Response
	 *
	 * Route::get('blogposts/delete/{id}', 'BlogPostsController@destroy');
	 */
	public function destroy($id)
	{
		$blogposts = BlogPosts::findOrFail($id);
		$blogposts->delete();

		return redirect()->route('blogposts.index')->with('message', 'Item deleted successfully.');
	}

}

```

## Example crud_code_help.txt
```php

---------------------------------------------------------------------------
Relation: create method
---------------------------------------------------------------------------

public function store(Request $request){

$this->validate($request, [
	'commentext' => 'required|max:255',
	'ip' => 'required|max:15',
]);

$BlogPosts = BlogPosts::find($request->input('id'));
$BlogComments = $BlogPosts->BlogComments()->create([
	'commentext' => $request->input('commentext'),
	'ip'         => $request->input('ip'),
]);

return redirect()->route('blogposts.index')->with('message', 'Item created successfully.');
}

---------------------------------------------------------------------------
Relation: associate method
---------------------------------------------------------------------------

public function store(Request $request){

$this->validate($request, [
	'commentext' => 'required|max:255',
	'ip' => 'required|max:15',
]);

$BlogPosts = BlogPosts::find($request->input('id'));

$BlogComments = new BlogComments();
$BlogComments->commentext = $request->input('commentext');
$BlogComments->ip         = $request->input('ip');

$BlogComments->BlogPosts()->associate($BlogPosts);
$BlogComments->save();

return redirect()->route('blogposts.index')->with('message', 'Item created successfully.');
}

// ------  Read one data ------ 
$BlogPosts = BlogPosts::find(1);
echo $BlogPosts->id;
foreach ($BlogPosts->BlogComments as $data) {
   echo $data->ip;
}

//------- Read all data -------- 
$BlogPosts = BlogPosts::all();
foreach ($BlogPosts as $relate) {
    echo $relate->id;
    foreach($relate->BlogComments as $data){
       echo $data->ip;
    }
}

---------------------------------------------------------------------------
Relation: attach method
---------------------------------------------------------------------------

public function store(Request $request){

$this->validate($request, [
	'category_id' => 'required|numeric',
	'title' => 'required|max:255',
	'summary' => 'required|max:65535',
	'content' => 'required|max:65535',
	'slug' => 'required|unique:blog_posts|max:255',
	'comments' => 'required|numeric',
]);

$BlogTags = BlogTags::find($request->input('id'));

$BlogPosts = new BlogPosts();
$BlogPosts->category_id = $request->input('category_id');
$BlogPosts->title       = $request->input('title');
$BlogPosts->summary     = $request->input('summary');
$BlogPosts->content     = $request->input('content');
$BlogPosts->slug        = $request->input('slug');
$BlogPosts->comments    = $request->input('comments');
$BlogPosts->save();
$BlogTags->BlogPosts()->attach($BlogPosts->id);

return redirect()->route('blogposts.index')->with('message', 'Item created successfully.');
}

---------------------------------------------------------------------------
Relation: attach method
---------------------------------------------------------------------------

public function store(Request $request){

$this->validate($request, [
	'name' => 'required|max:255',
	'slug' => 'required|unique:blog_tags|max:255',
]);

$BlogPosts = BlogPosts::find($request->input('id'));

$BlogTags = new BlogTags();
$BlogTags->name = $request->input('name');
$BlogTags->slug = $request->input('slug');
$BlogTags->save();
$BlogPosts->BlogTags()->attach($BlogTags->id);

return redirect()->route('blogtags.index')->with('message', 'Item created successfully.');
}

---------------------------------------------------------------------------
Relation: create method
---------------------------------------------------------------------------

public function store(Request $request){

$this->validate($request, [
	'title' => 'required|max:255',
	'summary' => 'required|max:65535',
	'content' => 'required|max:65535',
	'slug' => 'required|unique:blog_posts|max:255',
	'comments' => 'required|numeric',
]);

$BlogCategories = BlogCategories::find($request->input('id'));
$BlogPosts = $BlogCategories->BlogPosts()->create([
	'title'    => $request->input('title'),
	'summary'  => $request->input('summary'),
	'content'  => $request->input('content'),
	'slug'     => $request->input('slug'),
	'comments' => $request->input('comments'),
]);

return redirect()->route('blogcategories.index')->with('message', 'Item created successfully.');
}

---------------------------------------------------------------------------
Relation: associate method
---------------------------------------------------------------------------

public function store(Request $request){

$this->validate($request, [
	'title' => 'required|max:255',
	'summary' => 'required|max:65535',
	'content' => 'required|max:65535',
	'slug' => 'required|unique:blog_posts|max:255',
	'comments' => 'required|numeric',
]);

$BlogCategories = BlogCategories::find($request->input('id'));

$BlogPosts = new BlogPosts();
$BlogPosts->title    = $request->input('title');
$BlogPosts->summary  = $request->input('summary');
$BlogPosts->content  = $request->input('content');
$BlogPosts->slug     = $request->input('slug');
$BlogPosts->comments = $request->input('comments');

$BlogPosts->BlogCategories()->associate($BlogCategories);
$BlogPosts->save();

return redirect()->route('blogcategories.index')->with('message', 'Item created successfully.');
}

// ------  Read one data ------ 
$BlogCategories = BlogCategories::find(1);
echo $BlogCategories->id;
foreach ($BlogCategories->BlogPosts as $data) {
   echo $data->comments;
}

//------- Read all data -------- 
$BlogCategories = BlogCategories::all();
foreach ($BlogCategories as $relate) {
    echo $relate->id;
    foreach($relate->BlogPosts as $data){
       echo $data->comments;
    }
}



```

## The sql tables in my database

```sql
--
-- Table structure for table `blog_categories`
--

CREATE TABLE `blog_categories` (
  `id` int(10) UNSIGNED NOT NULL PRIMARY KEY AUTO_INCREMENT,
  `name` varchar(255) NOT NULL,
  `slug` varchar(255) NOT NULL,
  `description` varchar(255) DEFAULT NULL,
  UNIQUE KEY `blog_categories_slug_unique`(`slug`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

INSERT INTO `blog_categories` (`id`, `name`, `slug`, `description`) VALUES
(1, 'Test category - 1', 'test-category-1', 'Test category - 1 Meta Desc'),
(2, 'Test category - 2', 'test-category-2', 'Test category - 2 Meta Desc'),
(3, 'Test category - 3', 'test-category-3', 'Test category - 3 Meta Desc'),
(4, 'Test category - 4', 'test-category-4', 'Test category - 4 Meta Desc');

--
-- Table structure for table `blog_posts`
--

CREATE TABLE `blog_posts` (
  `id` INT(10) UNSIGNED NOT NULL PRIMARY KEY AUTO_INCREMENT,
  `category_id` INT(10) UNSIGNED NOT NULL,
  `title` varchar(255) NOT NULL,
  `summary` text NOT NULL,
  `content` text NOT NULL,
  `slug` varchar(255) NOT NULL,
  `status` enum('draft','publish') NOT NULL DEFAULT 'publish',
  `comments` tinyint(1) NOT NULL DEFAULT '0',
  `created_at` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  UNIQUE KEY `blog_posts_slug_unique` (`slug`),
  KEY `blog_posts_category_id_index` (`category_id`),
  CONSTRAINT `fk_blog_post_blog_categories`
		FOREIGN KEY (category_id) REFERENCES blog_categories (id)
		ON DELETE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

INSERT INTO `blog_posts` (`id`, `category_id`, `title`,  `summary`, `content`, `slug`) VALUES
(1, 1, 'Test information - 1', 'Test information - summary - 1', 'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Curabitur varius eros ut ornare tempus. Cras a ligula lectus. Pellentesque eget tempor arcu. Proin nisl mi, auctor sit amet ornare vitae, egestas sit amet ante. Phasellus sit amet lobortis risus. Nam consectetur nisi consectetur aliquet condimentum. Morbi eu lacus in neque bibendum ultricies vel in risus.', 'test-information-1'),
(2, 2, 'Test information - 2', 'Test information - summary - 2', 'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Curabitur varius eros ut ornare tempus. Cras a ligula lectus. Pellentesque eget tempor arcu. Proin nisl mi, auctor sit amet ornare vitae, egestas sit amet ante. Phasellus sit amet lobortis risus. Nam consectetur nisi consectetur aliquet condimentum. Morbi eu lacus in neque bibendum ultricies vel in risus.', 'test-information-2'),
(3, 1, 'Test information - 3', 'Test information - summary - 3', 'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Curabitur varius eros ut ornare tempus. Cras a ligula lectus. Pellentesque eget tempor arcu. Proin nisl mi, auctor sit amet ornare vitae, egestas sit amet ante. Phasellus sit amet lobortis risus. Nam consectetur nisi consectetur aliquet condimentum. Morbi eu lacus in neque bibendum ultricies vel in risus.', 'test-information-3'),
(4, 4, 'Test information - 4', 'Test information - summary - 4', 'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Curabitur varius eros ut ornare tempus. Cras a ligula lectus. Pellentesque eget tempor arcu. Proin nisl mi, auctor sit amet ornare vitae, egestas sit amet ante. Phasellus sit amet lobortis risus. Nam consectetur nisi consectetur aliquet condimentum. Morbi eu lacus in neque bibendum ultricies vel in risus.', 'test-information-4');


--
-- Table structure for table `blog_comments`
--

CREATE TABLE `blog_comments` (
  `id` int(10) UNSIGNED NOT NULL PRIMARY KEY AUTO_INCREMENT,
  `commentext` varchar(255) NOT NULL,
  `post_id` INT(10) UNSIGNED NOT NULL,
  `ip` varchar(15) NOT NULL,
  `status` enum('pending','publish','spam') NOT NULL DEFAULT 'pending',
  `created_at` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
   KEY `blog_comments_post_id_index` (`post_id`),
   KEY `blog_comments_ip_index` (`ip`),
   CONSTRAINT `fk_blog_comments_blog_posts`
		FOREIGN KEY (post_id) REFERENCES blog_posts (id)
		ON DELETE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

INSERT INTO `blog_comments` (`id`, `commentext`, `post_id`, `ip`) 
VALUES (1, 'Client comment text ', 2, '127.0.0.1');

--
-- Table structure for table `blog_tags`
--

CREATE TABLE `blog_tags` (
  `id` int(10) UNSIGNED NOT NULL PRIMARY KEY AUTO_INCREMENT,
  `name` varchar(255) NOT NULL,
  `slug` varchar(255) NOT NULL,
  UNIQUE KEY `blog_tags_slug_unique` (`slug`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;


INSERT INTO `blog_tags` (`id`, `name`, `slug`) VALUES 
(1, 'client', 'client-text1'),
(2, 'test', 'test-text2'),
(3, 'summary', 'summary-text3'),
(4, 'ipsum', 'ipsum-text4');


--
-- Table structure for table `blog_post_tag`
--

CREATE TABLE `blog_post_tag` (
  `id` int(10) UNSIGNED NOT NULL PRIMARY KEY AUTO_INCREMENT,
  `tag_id` INT(10) UNSIGNED NOT NULL,
  `post_id` INT(10) UNSIGNED NOT NULL,
   KEY `blog_post_tag_tag_id_index` (`tag_id`),
   KEY `blog_post_tag_post_id_index` (`post_id`),
   CONSTRAINT `fk_blog_post_tag_blog_posts`
		FOREIGN KEY (post_id) REFERENCES blog_posts (id)
		ON DELETE CASCADE,
   CONSTRAINT `fk_blog_post_tag_blog_tags`
		FOREIGN KEY (tag_id) REFERENCES blog_tags (id)
		ON DELETE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

INSERT INTO `blog_post_tag` (`id`, `tag_id`, `post_id`) VALUES 
(1, 1, 1),
(2, 2, 2),
(3, 3, 3),
(4, 4, 4);
```




