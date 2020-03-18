# MVC-step-by-step
## Create a new page in framework project, web bootcamp 2, lesson 13, first 25 minutes

### Create a page Articles

1) app/Global/header.php
```
        <li class="nav-item">
            <a class="nav-link" href="?page=articles">Articles</a>
        </li>
```
2) app/Controllers/ , new file ArticlesController.php created
```
  class ArticlesController
{
    public function index(): string
    {
        return BaseView::generate('Articles', 'index');
    }
}
```
3) public/index.php, at switch
```
    case 'articles': // case name must match with href="?page=articles'
        echo (new ArticlesController)->index();
        break;
```
4) same page: require_once '../app/Controllers/ArticlesController.php';

5) app/Views/ , new folder Articles, new file index.php // visual page
    app/Views/Articles/index.php
```
    <h2></h2>
```
6) app/Repositories/ , new repository file ArticlesRepository.php created
```
class ArticlesRepository
{
    public static function getArticles(): array
    {
        return []; // will return an array
    }
}
```
7) app/data/ , new file articles.json created

copied actual data (from posts.json)

8) app/Repositories/ArticlesRepository.php
```
class ArticlesRepository
{
    public static function getArticles(): array
    {
        $data = file_get_contents(__DIR__ . '/../data/articles.json');
        return json_decode($data, true);
    }
}
```
9) app/Controllers/ArticlesController.php
```
class ArticlesController
{
    public function index(): string
    {
        $articles = ArticlesRepository::getArticles();
        return BaseView::generate('Articles', 'index', ['articles' => $articles]);
    }
}
```
10) public/index.php 
```
require_once '../app/Repositories/ArticlesRepository.php';
```
11) Views/Articles/index.php , inserts list structure
```
<ul>
<?php
foreach ($articles as $article) {
    echo '<li><a href="?page=article&id=' . $article['id'] . '">' . $article['title'] . '</a></li>';
}
?>
</ul>
```
12) public/index.php , new case created
```
    case 'article':
        echo (new ArticlesController)->viewArticle();
        break;
```
13) app/Controllers/ArticlesController.php , new function viewArticle() created
```
    public function viewArticle(): string
    {
        $id = $_GET['id'] ?? 0; // return ID or null
        $article = ArticlesRepository::getArticle($id);

        return BaseView::generate('Articles', 'viewArticle');
    }
```
14) app/Repositories/ArticlesRepository.php , define function getArticle()
```
 public static function getArticle(int $id): ?array
    {
        $articles = self::getArticles(); // get particular article: iterates through all articles and compares id-s
        foreach ($articles as $article) {
            if ($article['id'] == $id) {
                return $article;
            }
        }

        return null;
    }
```
15) app/Controllers/ArticlesController.php

in viewArticle():
```
return BaseView::generate('Articles', 'viewArticle', ['article' => $article]);
```
16) Views/Articles/ , new file viewArticle.php created , article structure with title and text
```
<h2><?php echo $article['title']; ?></h2>
<p><?= $article['text']; ?></p>
```
