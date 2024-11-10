test的逻辑就是 检验***实际***是否符合***预期***
factory: blueprint 
FactoryBot.create(:owner, user: @alex_user)
owner --> factory :owner do

validates = built in validator
validate = we build it ourselves
private method --> method only for internal business purposes

business logic -- model (just throw an error out)
process logic -- controller (decides how to handle the error)

.build = create object in memory and in database

`rails db:contexts`
`rails db:drop`
`rails db:create`
`rails db:migrate`