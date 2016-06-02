# BLOG.SH.GG

## Wulian Team Blog

[![Donate](https://img.shields.io/gratipay/willin.svg)](https://gratipay.com/willin/)  [![Build](https://travis-ci.org/WulianCC/WulianCC.github.io.svg)](https://travis-ci.org/WulianCC/WulianCC.github.io)  [![Dependency](https://david-dm.org/wuliancc/WulianCC.github.io.svg)](https://david-dm.org/wuliancc/WulianCC.github.io)

### Categories And Tags

Categories and Tags should use single English words, initials in capitals.

A post can have only `One` Category and `Several` Tags.

Categories should be within this list:

* Data (for Database and Big Data)
* Design
* Dev
* Diary (for things not related to work)
* Doing (for News or Announcement)

Tags can be Anything Common like these below:

* AI (for Artificial Intelligence and Deep Learning)
* FE (for Front-End)
* Life
* Note
* Product
* Tech
* Works

### Instructions

#### Create a Draft

```
hexo new 'UserNickName: Title Name' -slug 'nickname-title-name'
```

#### Publish a Draft

```
hexo publish nickname-title-name
```

#### Create a Post directly

```
hexo new post 'UserNickName: Title Name' -slug 'nickname-title-name'
```

### Deploy _cautiously use_

```
hexo deploy
```

Or try this better way:

```
make publish
```

Now we no need do anything to deploy the site thanks to **Travis-CI**.
