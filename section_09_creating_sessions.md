## 66. Sessions

***

## 67. Universally unique identifier - `UUID`

```
$ cd /Users/marshad/Desktop/golang-web-dev/030_sessions/01_uuid
$ go mod init 01_uuid
$ go mod tidy
``` 

***

## 68. Your first session

```go
type User struct {
	UserName string
	First    string
	Last     string
}
```

* `Session` has `{session_id, user_id}`
* `user_id` -> determines a unique `User`

```
var dbSessions = map[string]string{}        # composite literal
var dbSessions = make(map[string]string)    # same effect as above
```
***


## 69. Sign-up

```
<form method="post">
    <input type="email" name="username"  placeholder="email">      <br>
    <input type="text"  name="password"  placeholder="password">   <br>
    <input type="text"  name="firstname" placeholder="first name"> <br>
    <input type="text"  name="lastname"  placeholder="last name">  <br>
    <input type="submit">
</form>
```

***

## 70. Encrypt password with `bcrypt`

***

## 71. Login

***

## 72. Logout

***

## 73. Permissions

***

## 74. Expire session

***