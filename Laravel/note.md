# 日期时间处理包 Carbon
- https://learnku.com/articles/18085
- https://carbon.nesbot.com/docs/
# 数据填充 Faker
- https://github.com/fzaninotto/Faker
- https://laravelacademy.org/post/7053.html
```php
php artisan tinker
>>> factory(App\Post::class,20)->create();
```
# Laravel 基于 Scout 配置实现 Elasticsearch
- https://segmentfault.com/a/1190000019465503
- https://segmentfault.com/a/1190000019467183
- https://blog.csdn.net/jam00/article/details/52983056

# Laravel模型关联
- 一对一 hasOne （用户 - 手机号）
- 一对多 hasMany （文章 - 评论）
- 一对多反向 belongsTo (评论 - 文章)
- 多对多 belongsToMany （用户 - 角色）
- 远层一对多 hasManyThrough（国家 - 作者 - 文章）
- 多态关联 morphMany (文章/视频 - 评论)
- 多态多对多 morphToMany (文章/视频 - 标签)
