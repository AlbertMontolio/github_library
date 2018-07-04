learn capybara

# avoid entering passphrase in the terminal when pushing to github

```
Once you have started the SSH agent with:

eval $(ssh-agent)
You have to add your private key to it:

ssh-add
This will ask you your passphrase just once, and then you should be allowed to push, provided that you uploaded the public key to Github.

To save key permanently:

ssh-add -K  
This will persist it after you close and re-open it by storing it in user's keychain.
```

how to kill ports. restart the computer
this bla bla .pid file

after installing rails gem, you have to close the terminal


why to use get or post in second exercice, while submitting a form

```html
<form action="/tasks" method="post">
  <%= hidden_field_tag :authenticity_token, form_authenticity_token %>
  <label for="title">Title: </label>
  <input type="text" name="task[title]" id="title">

  <label for="description">description: </label>
  <input type="text" name="task[description]" id="description">

  <input type="submit">
</form>
```

if you put get, you show the token in the url!!!

how to pass a hidden_value in a form


https://github.com/thoughtbot/administrate

rspec















