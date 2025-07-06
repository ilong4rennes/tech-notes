PATS dataset
问题：不是每个用户都按照格式输入手机号
rails method: number_to_phone
![[Screen Shot 2024-02-15 at 02.33.03.png]]

validation process:
every method starts with validate / validates
if show error, the model stops the process (model, controller)
create record using insert command
post
update records: put / patch

after validation before save, save = create + update

reformat_phone：从一堆东西中筛选出10位数字

callback:
- before_save 这些东西 加在:method_name前面

only models have callbacks

test/sets
可以看到某些数据，比如看default库存
每次run test，empty database got populated
end test, drop the database

每次开多少药，by default--继续去sets里面看 哈哈哈
@amoxicillin.reload 为什么开完药要reload药？
memory和database里有时候不match，这个方法是让他俩match起来的方式
just to be safe
callback method --> need to reload (generally)!
reload just before run the test

in a class -- add some scope
scope return的是array of object

before_destroy -- 
delete -- delete without safety
destroy -- run all of the callbacks before deleting