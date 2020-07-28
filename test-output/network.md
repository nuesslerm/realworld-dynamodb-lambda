```
DELETE /__TESTUTILS__/purge
```
```
200 OK

"Purged all data!"
```
# Article
```
POST /users

{
  "user": {
    "email": "author-k8rp4r@email.com",
    "username": "author-k8rp4r",
    "password": "password"
  }
}
```
```
200 OK

{
  "user": {
    "email": "author-k8rp4r@email.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImF1dGhvci1rOHJwNHIiLCJpYXQiOjE1OTU4OTQ5ODQsImV4cCI6MTU5NjA2Nzc4NH0.AcWkWHU28i_TfjveeOFOKf_hkWalA7cGS4NLJqrEE_A",
    "username": "author-k8rp4r",
    "bio": "",
    "image": ""
  }
}
```
```
POST /users

{
  "user": {
    "email": "authoress-zekok9@email.com",
    "username": "authoress-zekok9",
    "password": "password"
  }
}
```
```
200 OK

{
  "user": {
    "email": "authoress-zekok9@email.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImF1dGhvcmVzcy16ZWtvazkiLCJpYXQiOjE1OTU4OTQ5ODQsImV4cCI6MTU5NjA2Nzc4NH0.HHLbMsULxp9pV7alHCqhlKoR4HVBs4uB4qxat75Un6g",
    "username": "authoress-zekok9",
    "bio": "",
    "image": ""
  }
}
```
```
POST /users

{
  "user": {
    "email": "non-author-utj5dc@email.com",
    "username": "non-author-utj5dc",
    "password": "password"
  }
}
```
```
200 OK

{
  "user": {
    "email": "non-author-utj5dc@email.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6Im5vbi1hdXRob3ItdXRqNWRjIiwiaWF0IjoxNTk1ODk0OTg0LCJleHAiOjE1OTYwNjc3ODR9.9au4M626LHLxMwruGR6-t34zJT6fPiEW2O60qxDjEGg",
    "username": "non-author-utj5dc",
    "bio": "",
    "image": ""
  }
}
```
## Create
### should create article
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body"
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-r38xud",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1595894984358,
    "updatedAt": 1595894984358,
    "author": {
      "username": "author-k8rp4r",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
### should create article with tags
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "tag_a",
      "tag_b"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-b4uhjg",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1595894984400,
    "updatedAt": 1595894984400,
    "author": {
      "username": "author-k8rp4r",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "tag_a",
      "tag_b"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
### should disallow unauthenticated user
```
POST /articles

{}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Must be logged in."
    ]
  }
}
```
### should enforce required fields
```
POST /articles

{}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Article must be specified."
    ]
  }
}
```
```
POST /articles

{
  "article": {}
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "title must be specified."
    ]
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "description must be specified."
    ]
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "body must be specified."
    ]
  }
}
```
## Get
### should get article by slug
```
GET /articles/title-r38xud
```
```
200 OK

{
  "article": {
    "createdAt": 1595894984358,
    "author": {
      "username": "author-k8rp4r",
      "bio": "",
      "image": "",
      "following": false
    },
    "description": "description",
    "title": "title",
    "body": "body",
    "slug": "title-r38xud",
    "updatedAt": 1595894984358,
    "tagList": [],
    "favoritesCount": 0,
    "favorited": false
  }
}
```
### should get article with tags by slug
```
GET /articles/title-b4uhjg
```
```
200 OK

{
  "article": {
    "tagList": [
      "tag_a",
      "tag_b"
    ],
    "createdAt": 1595894984400,
    "author": {
      "username": "author-k8rp4r",
      "bio": "",
      "image": "",
      "following": false
    },
    "description": "description",
    "title": "title",
    "body": "body",
    "slug": "title-b4uhjg",
    "updatedAt": 1595894984400,
    "favoritesCount": 0,
    "favorited": false
  }
}
```
### should disallow unknown slug
```
GET /articles/58u21n
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Article not found: [58u21n]"
    ]
  }
}
```
## Update
### should update article
```
PUT /articles/title-b4uhjg

{
  "article": {
    "title": "newtitle"
  }
}
```
```
200 OK

{
  "article": {
    "tagList": [
      "tag_a",
      "tag_b"
    ],
    "createdAt": 1595894984400,
    "author": {
      "username": "author-k8rp4r",
      "bio": "",
      "image": "",
      "following": false
    },
    "description": "description",
    "title": "newtitle",
    "body": "body",
    "slug": "title-b4uhjg",
    "updatedAt": 1595894984400,
    "favoritesCount": 0,
    "favorited": false
  }
}
```
```
PUT /articles/title-b4uhjg

{
  "article": {
    "description": "newdescription"
  }
}
```
```
200 OK

{
  "article": {
    "tagList": [
      "tag_a",
      "tag_b"
    ],
    "createdAt": 1595894984400,
    "author": {
      "username": "author-k8rp4r",
      "bio": "",
      "image": "",
      "following": false
    },
    "description": "newdescription",
    "title": "newtitle",
    "body": "body",
    "slug": "title-b4uhjg",
    "updatedAt": 1595894984400,
    "favoritesCount": 0,
    "favorited": false
  }
}
```
```
PUT /articles/title-b4uhjg

{
  "article": {
    "body": "newbody"
  }
}
```
```
200 OK

{
  "article": {
    "tagList": [
      "tag_a",
      "tag_b"
    ],
    "createdAt": 1595894984400,
    "author": {
      "username": "author-k8rp4r",
      "bio": "",
      "image": "",
      "following": false
    },
    "description": "newdescription",
    "title": "newtitle",
    "body": "newbody",
    "slug": "title-b4uhjg",
    "updatedAt": 1595894984400,
    "favoritesCount": 0,
    "favorited": false
  }
}
```
### should disallow missing mutation
```
PUT /articles/title-b4uhjg

{}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Article mutation must be specified."
    ]
  }
}
```
### should disallow empty mutation
```
PUT /articles/title-b4uhjg

{
  "article": {}
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "At least one field must be specified: [title, description, article]."
    ]
  }
}
```
### should disallow unauthenticated update
```
PUT /articles/title-b4uhjg

{
  "article": {
    "title": "newtitle"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Must be logged in."
    ]
  }
}
```
### should disallow updating non-existent article
```
PUT /articles/foo-title-b4uhjg

{
  "article": {
    "title": "newtitle"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Article not found: [foo-title-b4uhjg]"
    ]
  }
}
```
### should disallow non-author from updating
```
PUT /articles/title-b4uhjg

{
  "article": {
    "title": "newtitle"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Article can only be updated by author: [author-k8rp4r]"
    ]
  }
}
```
## Favorite
### should favorite article
```
POST /articles/title-r38xud/favorite

{}
```
```
200 OK

{
  "article": {
    "createdAt": 1595894984358,
    "author": {
      "username": "author-k8rp4r",
      "bio": "",
      "image": "",
      "following": false
    },
    "description": "description",
    "title": "title",
    "body": "body",
    "slug": "title-r38xud",
    "updatedAt": 1595894984358,
    "favoritedBy": [
      "non-author-utj5dc"
    ],
    "favoritesCount": 1,
    "tagList": [],
    "favorited": true
  }
}
```
```
GET /articles/title-r38xud
```
```
200 OK

{
  "article": {
    "createdAt": 1595894984358,
    "author": {
      "username": "author-k8rp4r",
      "bio": "",
      "image": "",
      "following": false
    },
    "description": "description",
    "title": "title",
    "body": "body",
    "favoritesCount": 1,
    "slug": "title-r38xud",
    "updatedAt": 1595894984358,
    "tagList": [],
    "favorited": true
  }
}
```
### should disallow favoriting by unauthenticated user
```
POST /articles/title-r38xud/favorite

{}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Must be logged in."
    ]
  }
}
```
### should disallow favoriting unknown article
```
POST /articles/title-r38xud_foo/favorite

{}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Article not found: [title-r38xud_foo]"
    ]
  }
}
```
### should unfavorite article
```
DELETE /articles/title-r38xud/favorite
```
```
200 OK

{
  "article": {
    "createdAt": 1595894984358,
    "author": {
      "username": "author-k8rp4r",
      "bio": "",
      "image": "",
      "following": false
    },
    "description": "description",
    "title": "title",
    "body": "body",
    "favoritesCount": 0,
    "slug": "title-r38xud",
    "updatedAt": 1595894984358,
    "tagList": [],
    "favorited": false
  }
}
```
## Delete
### should delete article
```
DELETE /articles/title-r38xud
```
```
200 OK

{}
```
```
GET /articles/title-r38xud
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Article not found: [title-r38xud]"
    ]
  }
}
```
### should disallow deleting by unauthenticated user
```
DELETE /articles/foo
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Must be logged in."
    ]
  }
}
```
### should disallow deleting unknown article
```
DELETE /articles/foobar
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Article not found: [foobar]"
    ]
  }
}
```
### should disallow deleting article by non-author
```
DELETE /articles/title-b4uhjg
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Article can only be deleted by author: [author-k8rp4r]"
    ]
  }
}
```
## List
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "nhxsv9",
      "tag_0",
      "tag_mod_2_0",
      "tag_mod_3_0"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-qk3qx5",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1595894985488,
    "updatedAt": 1595894985488,
    "author": {
      "username": "authoress-zekok9",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "nhxsv9",
      "tag_0",
      "tag_mod_2_0",
      "tag_mod_3_0"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "z31d4o",
      "tag_1",
      "tag_mod_2_1",
      "tag_mod_3_1"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-qtj7ks",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1595894985540,
    "updatedAt": 1595894985540,
    "author": {
      "username": "author-k8rp4r",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "z31d4o",
      "tag_1",
      "tag_mod_2_1",
      "tag_mod_3_1"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "2jbu5o",
      "tag_2",
      "tag_mod_2_0",
      "tag_mod_3_2"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-opociz",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1595894985571,
    "updatedAt": 1595894985571,
    "author": {
      "username": "authoress-zekok9",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "2jbu5o",
      "tag_2",
      "tag_mod_2_0",
      "tag_mod_3_2"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "kxecyp",
      "tag_3",
      "tag_mod_2_1",
      "tag_mod_3_0"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-w9hcne",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1595894985599,
    "updatedAt": 1595894985599,
    "author": {
      "username": "author-k8rp4r",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "kxecyp",
      "tag_3",
      "tag_mod_2_1",
      "tag_mod_3_0"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "qlzykw",
      "tag_4",
      "tag_mod_2_0",
      "tag_mod_3_1"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-cw5q3p",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1595894985630,
    "updatedAt": 1595894985630,
    "author": {
      "username": "authoress-zekok9",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "qlzykw",
      "tag_4",
      "tag_mod_2_0",
      "tag_mod_3_1"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "g4fbp2",
      "tag_5",
      "tag_mod_2_1",
      "tag_mod_3_2"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-x8hcos",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1595894985656,
    "updatedAt": 1595894985656,
    "author": {
      "username": "author-k8rp4r",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "g4fbp2",
      "tag_5",
      "tag_mod_2_1",
      "tag_mod_3_2"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "h6iffs",
      "tag_6",
      "tag_mod_2_0",
      "tag_mod_3_0"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-t4m477",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1595894985688,
    "updatedAt": 1595894985688,
    "author": {
      "username": "authoress-zekok9",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "h6iffs",
      "tag_6",
      "tag_mod_2_0",
      "tag_mod_3_0"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "v9rlqe",
      "tag_7",
      "tag_mod_2_1",
      "tag_mod_3_1"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-zb7g58",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1595894985714,
    "updatedAt": 1595894985714,
    "author": {
      "username": "author-k8rp4r",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "v9rlqe",
      "tag_7",
      "tag_mod_2_1",
      "tag_mod_3_1"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "f17j9g",
      "tag_8",
      "tag_mod_2_0",
      "tag_mod_3_2"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-3zkjpf",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1595894985741,
    "updatedAt": 1595894985741,
    "author": {
      "username": "authoress-zekok9",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "f17j9g",
      "tag_8",
      "tag_mod_2_0",
      "tag_mod_3_2"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "x7nv7c",
      "tag_9",
      "tag_mod_2_1",
      "tag_mod_3_0"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-en624n",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1595894985778,
    "updatedAt": 1595894985778,
    "author": {
      "username": "author-k8rp4r",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "x7nv7c",
      "tag_9",
      "tag_mod_2_1",
      "tag_mod_3_0"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "nhn4bq",
      "tag_10",
      "tag_mod_2_0",
      "tag_mod_3_1"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-2vpwa6",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1595894985812,
    "updatedAt": 1595894985812,
    "author": {
      "username": "authoress-zekok9",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "nhn4bq",
      "tag_10",
      "tag_mod_2_0",
      "tag_mod_3_1"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "3fkohw",
      "tag_11",
      "tag_mod_2_1",
      "tag_mod_3_2"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-inei34",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1595894985838,
    "updatedAt": 1595894985838,
    "author": {
      "username": "author-k8rp4r",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "3fkohw",
      "tag_11",
      "tag_mod_2_1",
      "tag_mod_3_2"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "v4ed71",
      "tag_12",
      "tag_mod_2_0",
      "tag_mod_3_0"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-qmrhtj",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1595894985863,
    "updatedAt": 1595894985863,
    "author": {
      "username": "authoress-zekok9",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "v4ed71",
      "tag_12",
      "tag_mod_2_0",
      "tag_mod_3_0"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "26xvjr",
      "tag_13",
      "tag_mod_2_1",
      "tag_mod_3_1"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-z9whui",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1595894985888,
    "updatedAt": 1595894985888,
    "author": {
      "username": "author-k8rp4r",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "26xvjr",
      "tag_13",
      "tag_mod_2_1",
      "tag_mod_3_1"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "l6vcd2",
      "tag_14",
      "tag_mod_2_0",
      "tag_mod_3_2"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-1m2j5w",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1595894985915,
    "updatedAt": 1595894985915,
    "author": {
      "username": "authoress-zekok9",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "l6vcd2",
      "tag_14",
      "tag_mod_2_0",
      "tag_mod_3_2"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "ij7ep0",
      "tag_15",
      "tag_mod_2_1",
      "tag_mod_3_0"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-asz8gg",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1595894985941,
    "updatedAt": 1595894985941,
    "author": {
      "username": "author-k8rp4r",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "ij7ep0",
      "tag_15",
      "tag_mod_2_1",
      "tag_mod_3_0"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "p4fqrt",
      "tag_16",
      "tag_mod_2_0",
      "tag_mod_3_1"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-gs9yza",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1595894985982,
    "updatedAt": 1595894985982,
    "author": {
      "username": "authoress-zekok9",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "p4fqrt",
      "tag_16",
      "tag_mod_2_0",
      "tag_mod_3_1"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "bfksaf",
      "tag_17",
      "tag_mod_2_1",
      "tag_mod_3_2"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-foocef",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1595894986007,
    "updatedAt": 1595894986007,
    "author": {
      "username": "author-k8rp4r",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "bfksaf",
      "tag_17",
      "tag_mod_2_1",
      "tag_mod_3_2"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "kqohxj",
      "tag_18",
      "tag_mod_2_0",
      "tag_mod_3_0"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-n8m86a",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1595894986035,
    "updatedAt": 1595894986035,
    "author": {
      "username": "authoress-zekok9",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "kqohxj",
      "tag_18",
      "tag_mod_2_0",
      "tag_mod_3_0"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body",
    "tagList": [
      "1x4f78",
      "tag_19",
      "tag_mod_2_1",
      "tag_mod_3_1"
    ]
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-y2f6ow",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1595894986083,
    "updatedAt": 1595894986083,
    "author": {
      "username": "author-k8rp4r",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [
      "1x4f78",
      "tag_19",
      "tag_mod_2_1",
      "tag_mod_3_1"
    ],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
### should list articles
```
GET /articles
```
```
200 OK

{
  "articles": [
    {
      "tagList": [
        "1x4f78",
        "tag_19",
        "tag_mod_2_1",
        "tag_mod_3_1"
      ],
      "createdAt": 1595894986083,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-y2f6ow",
      "updatedAt": 1595894986083,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "kqohxj",
        "tag_18",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1595894986035,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-n8m86a",
      "updatedAt": 1595894986035,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "bfksaf",
        "tag_17",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1595894986007,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-foocef",
      "updatedAt": 1595894986007,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "p4fqrt",
        "tag_16",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1595894985982,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-gs9yza",
      "updatedAt": 1595894985982,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "ij7ep0",
        "tag_15",
        "tag_mod_2_1",
        "tag_mod_3_0"
      ],
      "createdAt": 1595894985941,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-asz8gg",
      "updatedAt": 1595894985941,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "l6vcd2",
        "tag_14",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1595894985915,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-1m2j5w",
      "updatedAt": 1595894985915,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "26xvjr",
        "tag_13",
        "tag_mod_2_1",
        "tag_mod_3_1"
      ],
      "createdAt": 1595894985888,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-z9whui",
      "updatedAt": 1595894985888,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_12",
        "tag_mod_2_0",
        "tag_mod_3_0",
        "v4ed71"
      ],
      "createdAt": 1595894985863,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-qmrhtj",
      "updatedAt": 1595894985863,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "3fkohw",
        "tag_11",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1595894985838,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-inei34",
      "updatedAt": 1595894985838,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "nhn4bq",
        "tag_10",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1595894985812,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-2vpwa6",
      "updatedAt": 1595894985812,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_9",
        "tag_mod_2_1",
        "tag_mod_3_0",
        "x7nv7c"
      ],
      "createdAt": 1595894985778,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-en624n",
      "updatedAt": 1595894985778,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "f17j9g",
        "tag_8",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1595894985741,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-3zkjpf",
      "updatedAt": 1595894985741,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_7",
        "tag_mod_2_1",
        "tag_mod_3_1",
        "v9rlqe"
      ],
      "createdAt": 1595894985714,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-zb7g58",
      "updatedAt": 1595894985714,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "h6iffs",
        "tag_6",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1595894985688,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-t4m477",
      "updatedAt": 1595894985688,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "g4fbp2",
        "tag_5",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1595894985656,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-x8hcos",
      "updatedAt": 1595894985656,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "qlzykw",
        "tag_4",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1595894985630,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-cw5q3p",
      "updatedAt": 1595894985630,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "kxecyp",
        "tag_3",
        "tag_mod_2_1",
        "tag_mod_3_0"
      ],
      "createdAt": 1595894985599,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-w9hcne",
      "updatedAt": 1595894985599,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "2jbu5o",
        "tag_2",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1595894985571,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-opociz",
      "updatedAt": 1595894985571,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_1",
        "tag_mod_2_1",
        "tag_mod_3_1",
        "z31d4o"
      ],
      "createdAt": 1595894985540,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-qtj7ks",
      "updatedAt": 1595894985540,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "nhxsv9",
        "tag_0",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1595894985488,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-qk3qx5",
      "updatedAt": 1595894985488,
      "favoritesCount": 0,
      "favorited": false
    }
  ]
}
```
### should list articles with tag
```
GET /articles?tag=tag_7
```
```
200 OK

{
  "articles": [
    {
      "tagList": [
        "tag_7",
        "tag_mod_2_1",
        "tag_mod_3_1",
        "v9rlqe"
      ],
      "createdAt": 1595894985714,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-zb7g58",
      "updatedAt": 1595894985714,
      "favoritesCount": 0,
      "favorited": false
    }
  ]
}
```
```
GET /articles?tag=tag_mod_3_2
```
```
200 OK

{
  "articles": [
    {
      "tagList": [
        "bfksaf",
        "tag_17",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1595894986007,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-foocef",
      "updatedAt": 1595894986007,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "l6vcd2",
        "tag_14",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1595894985915,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-1m2j5w",
      "updatedAt": 1595894985915,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "3fkohw",
        "tag_11",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1595894985838,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-inei34",
      "updatedAt": 1595894985838,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "f17j9g",
        "tag_8",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1595894985741,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-3zkjpf",
      "updatedAt": 1595894985741,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "g4fbp2",
        "tag_5",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1595894985656,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-x8hcos",
      "updatedAt": 1595894985656,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "2jbu5o",
        "tag_2",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1595894985571,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-opociz",
      "updatedAt": 1595894985571,
      "favoritesCount": 0,
      "favorited": false
    }
  ]
}
```
### should list articles by author
```
GET /articles?author=authoress-zekok9
```
```
200 OK

{
  "articles": [
    {
      "tagList": [
        "kqohxj",
        "tag_18",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1595894986035,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-n8m86a",
      "updatedAt": 1595894986035,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "p4fqrt",
        "tag_16",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1595894985982,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-gs9yza",
      "updatedAt": 1595894985982,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "l6vcd2",
        "tag_14",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1595894985915,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-1m2j5w",
      "updatedAt": 1595894985915,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_12",
        "tag_mod_2_0",
        "tag_mod_3_0",
        "v4ed71"
      ],
      "createdAt": 1595894985863,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-qmrhtj",
      "updatedAt": 1595894985863,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "nhn4bq",
        "tag_10",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1595894985812,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-2vpwa6",
      "updatedAt": 1595894985812,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "f17j9g",
        "tag_8",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1595894985741,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-3zkjpf",
      "updatedAt": 1595894985741,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "h6iffs",
        "tag_6",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1595894985688,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-t4m477",
      "updatedAt": 1595894985688,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "qlzykw",
        "tag_4",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1595894985630,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-cw5q3p",
      "updatedAt": 1595894985630,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "2jbu5o",
        "tag_2",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1595894985571,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-opociz",
      "updatedAt": 1595894985571,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "nhxsv9",
        "tag_0",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1595894985488,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-qk3qx5",
      "updatedAt": 1595894985488,
      "favoritesCount": 0,
      "favorited": false
    }
  ]
}
```
### should list articles favorited by user
```
GET /articles?favorited=non-author-utj5dc
```
```
200 OK

{
  "articles": []
}
```
### should list articles by limit/offset
```
GET /articles?author=author-k8rp4r&limit=2
```
```
200 OK

{
  "articles": [
    {
      "tagList": [
        "1x4f78",
        "tag_19",
        "tag_mod_2_1",
        "tag_mod_3_1"
      ],
      "createdAt": 1595894986083,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-y2f6ow",
      "updatedAt": 1595894986083,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "bfksaf",
        "tag_17",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1595894986007,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-foocef",
      "updatedAt": 1595894986007,
      "favoritesCount": 0,
      "favorited": false
    }
  ]
}
```
```
GET /articles?author=author-k8rp4r&limit=2&offset=2
```
```
200 OK

{
  "articles": [
    {
      "tagList": [
        "ij7ep0",
        "tag_15",
        "tag_mod_2_1",
        "tag_mod_3_0"
      ],
      "createdAt": 1595894985941,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-asz8gg",
      "updatedAt": 1595894985941,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "26xvjr",
        "tag_13",
        "tag_mod_2_1",
        "tag_mod_3_1"
      ],
      "createdAt": 1595894985888,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-z9whui",
      "updatedAt": 1595894985888,
      "favoritesCount": 0,
      "favorited": false
    }
  ]
}
```
### should list articles when authenticated
```
GET /articles
```
```
200 OK

{
  "articles": [
    {
      "tagList": [
        "1x4f78",
        "tag_19",
        "tag_mod_2_1",
        "tag_mod_3_1"
      ],
      "createdAt": 1595894986083,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-y2f6ow",
      "updatedAt": 1595894986083,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "kqohxj",
        "tag_18",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1595894986035,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-n8m86a",
      "updatedAt": 1595894986035,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "bfksaf",
        "tag_17",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1595894986007,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-foocef",
      "updatedAt": 1595894986007,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "p4fqrt",
        "tag_16",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1595894985982,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-gs9yza",
      "updatedAt": 1595894985982,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "ij7ep0",
        "tag_15",
        "tag_mod_2_1",
        "tag_mod_3_0"
      ],
      "createdAt": 1595894985941,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-asz8gg",
      "updatedAt": 1595894985941,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "l6vcd2",
        "tag_14",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1595894985915,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-1m2j5w",
      "updatedAt": 1595894985915,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "26xvjr",
        "tag_13",
        "tag_mod_2_1",
        "tag_mod_3_1"
      ],
      "createdAt": 1595894985888,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-z9whui",
      "updatedAt": 1595894985888,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_12",
        "tag_mod_2_0",
        "tag_mod_3_0",
        "v4ed71"
      ],
      "createdAt": 1595894985863,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-qmrhtj",
      "updatedAt": 1595894985863,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "3fkohw",
        "tag_11",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1595894985838,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-inei34",
      "updatedAt": 1595894985838,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "nhn4bq",
        "tag_10",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1595894985812,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-2vpwa6",
      "updatedAt": 1595894985812,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_9",
        "tag_mod_2_1",
        "tag_mod_3_0",
        "x7nv7c"
      ],
      "createdAt": 1595894985778,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-en624n",
      "updatedAt": 1595894985778,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "f17j9g",
        "tag_8",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1595894985741,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-3zkjpf",
      "updatedAt": 1595894985741,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_7",
        "tag_mod_2_1",
        "tag_mod_3_1",
        "v9rlqe"
      ],
      "createdAt": 1595894985714,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-zb7g58",
      "updatedAt": 1595894985714,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "h6iffs",
        "tag_6",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1595894985688,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-t4m477",
      "updatedAt": 1595894985688,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "g4fbp2",
        "tag_5",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1595894985656,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-x8hcos",
      "updatedAt": 1595894985656,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "qlzykw",
        "tag_4",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1595894985630,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-cw5q3p",
      "updatedAt": 1595894985630,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "kxecyp",
        "tag_3",
        "tag_mod_2_1",
        "tag_mod_3_0"
      ],
      "createdAt": 1595894985599,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-w9hcne",
      "updatedAt": 1595894985599,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "2jbu5o",
        "tag_2",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1595894985571,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-opociz",
      "updatedAt": 1595894985571,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_1",
        "tag_mod_2_1",
        "tag_mod_3_1",
        "z31d4o"
      ],
      "createdAt": 1595894985540,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-qtj7ks",
      "updatedAt": 1595894985540,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "nhxsv9",
        "tag_0",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1595894985488,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": false
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-qk3qx5",
      "updatedAt": 1595894985488,
      "favoritesCount": 0,
      "favorited": false
    }
  ]
}
```
### should disallow multiple of author/tag/favorited
```
GET /articles?tag=foo&author=bar
```
```
GET /articles?author=foo&favorited=bar
```
```
GET /articles?favorited=foo&tag=bar
```
## Feed
### should get feed
```
GET /articles/feed
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Only one of these can be specified: [tag, author, favorited]"
    ]
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Only one of these can be specified: [tag, author, favorited]"
    ]
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Only one of these can be specified: [tag, author, favorited]"
    ]
  }
}
```
```
200 OK

{
  "articles": []
}
```
```
POST /profiles/authoress-zekok9/follow

{}
```
```
200 OK

{
  "profile": {
    "username": "authoress-zekok9",
    "bio": "",
    "image": "",
    "following": true
  }
}
```
```
GET /articles/feed
```
```
200 OK

{
  "articles": [
    {
      "tagList": [
        "kqohxj",
        "tag_18",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1595894986035,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-n8m86a",
      "updatedAt": 1595894986035,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "p4fqrt",
        "tag_16",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1595894985982,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-gs9yza",
      "updatedAt": 1595894985982,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "l6vcd2",
        "tag_14",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1595894985915,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-1m2j5w",
      "updatedAt": 1595894985915,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_12",
        "tag_mod_2_0",
        "tag_mod_3_0",
        "v4ed71"
      ],
      "createdAt": 1595894985863,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-qmrhtj",
      "updatedAt": 1595894985863,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "nhn4bq",
        "tag_10",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1595894985812,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-2vpwa6",
      "updatedAt": 1595894985812,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "f17j9g",
        "tag_8",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1595894985741,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-3zkjpf",
      "updatedAt": 1595894985741,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "h6iffs",
        "tag_6",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1595894985688,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-t4m477",
      "updatedAt": 1595894985688,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "qlzykw",
        "tag_4",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1595894985630,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-cw5q3p",
      "updatedAt": 1595894985630,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "2jbu5o",
        "tag_2",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1595894985571,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-opociz",
      "updatedAt": 1595894985571,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "nhxsv9",
        "tag_0",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1595894985488,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-qk3qx5",
      "updatedAt": 1595894985488,
      "favoritesCount": 0,
      "favorited": false
    }
  ]
}
```
```
POST /profiles/author-k8rp4r/follow

{}
```
```
200 OK

{
  "profile": {
    "username": "author-k8rp4r",
    "bio": "",
    "image": "",
    "following": true
  }
}
```
```
GET /articles/feed
```
```
200 OK

{
  "articles": [
    {
      "tagList": [
        "1x4f78",
        "tag_19",
        "tag_mod_2_1",
        "tag_mod_3_1"
      ],
      "createdAt": 1595894986083,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-y2f6ow",
      "updatedAt": 1595894986083,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "kqohxj",
        "tag_18",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1595894986035,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-n8m86a",
      "updatedAt": 1595894986035,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "bfksaf",
        "tag_17",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1595894986007,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-foocef",
      "updatedAt": 1595894986007,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "p4fqrt",
        "tag_16",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1595894985982,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-gs9yza",
      "updatedAt": 1595894985982,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "ij7ep0",
        "tag_15",
        "tag_mod_2_1",
        "tag_mod_3_0"
      ],
      "createdAt": 1595894985941,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-asz8gg",
      "updatedAt": 1595894985941,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "l6vcd2",
        "tag_14",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1595894985915,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-1m2j5w",
      "updatedAt": 1595894985915,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "26xvjr",
        "tag_13",
        "tag_mod_2_1",
        "tag_mod_3_1"
      ],
      "createdAt": 1595894985888,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-z9whui",
      "updatedAt": 1595894985888,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_12",
        "tag_mod_2_0",
        "tag_mod_3_0",
        "v4ed71"
      ],
      "createdAt": 1595894985863,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-qmrhtj",
      "updatedAt": 1595894985863,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "3fkohw",
        "tag_11",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1595894985838,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-inei34",
      "updatedAt": 1595894985838,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "nhn4bq",
        "tag_10",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1595894985812,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-2vpwa6",
      "updatedAt": 1595894985812,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_9",
        "tag_mod_2_1",
        "tag_mod_3_0",
        "x7nv7c"
      ],
      "createdAt": 1595894985778,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-en624n",
      "updatedAt": 1595894985778,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "f17j9g",
        "tag_8",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1595894985741,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-3zkjpf",
      "updatedAt": 1595894985741,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_7",
        "tag_mod_2_1",
        "tag_mod_3_1",
        "v9rlqe"
      ],
      "createdAt": 1595894985714,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-zb7g58",
      "updatedAt": 1595894985714,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "h6iffs",
        "tag_6",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1595894985688,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-t4m477",
      "updatedAt": 1595894985688,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "g4fbp2",
        "tag_5",
        "tag_mod_2_1",
        "tag_mod_3_2"
      ],
      "createdAt": 1595894985656,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-x8hcos",
      "updatedAt": 1595894985656,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "qlzykw",
        "tag_4",
        "tag_mod_2_0",
        "tag_mod_3_1"
      ],
      "createdAt": 1595894985630,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-cw5q3p",
      "updatedAt": 1595894985630,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "kxecyp",
        "tag_3",
        "tag_mod_2_1",
        "tag_mod_3_0"
      ],
      "createdAt": 1595894985599,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-w9hcne",
      "updatedAt": 1595894985599,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "2jbu5o",
        "tag_2",
        "tag_mod_2_0",
        "tag_mod_3_2"
      ],
      "createdAt": 1595894985571,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-opociz",
      "updatedAt": 1595894985571,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "tag_1",
        "tag_mod_2_1",
        "tag_mod_3_1",
        "z31d4o"
      ],
      "createdAt": 1595894985540,
      "author": {
        "username": "author-k8rp4r",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-qtj7ks",
      "updatedAt": 1595894985540,
      "favoritesCount": 0,
      "favorited": false
    },
    {
      "tagList": [
        "nhxsv9",
        "tag_0",
        "tag_mod_2_0",
        "tag_mod_3_0"
      ],
      "createdAt": 1595894985488,
      "author": {
        "username": "authoress-zekok9",
        "bio": "",
        "image": "",
        "following": true
      },
      "description": "description",
      "title": "title",
      "body": "body",
      "slug": "title-qk3qx5",
      "updatedAt": 1595894985488,
      "favoritesCount": 0,
      "favorited": false
    }
  ]
}
```
### should disallow unauthenticated feed
```
GET /articles/feed
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Must be logged in."
    ]
  }
}
```
## Tags
### should get tags
```
GET /tags
```
```
200 OK

{
  "tags": [
    "kqohxj",
    "tag_18",
    "tag_mod_2_0",
    "tag_mod_3_0",
    "tag_1",
    "tag_mod_2_1",
    "tag_mod_3_1",
    "z31d4o",
    "f17j9g",
    "tag_8",
    "tag_mod_3_2",
    "h6iffs",
    "tag_6",
    "bfksaf",
    "tag_17",
    "p4fqrt",
    "tag_16",
    "tag_a",
    "tag_b",
    "qlzykw",
    "tag_4",
    "26xvjr",
    "tag_13",
    "g4fbp2",
    "tag_5",
    "nhn4bq",
    "tag_10",
    "tag_9",
    "x7nv7c",
    "2jbu5o",
    "tag_2",
    "tag_12",
    "v4ed71",
    "nhxsv9",
    "tag_0",
    "tag_7",
    "v9rlqe",
    "1x4f78",
    "tag_19",
    "kxecyp",
    "tag_3",
    "3fkohw",
    "tag_11",
    "l6vcd2",
    "tag_14",
    "ij7ep0",
    "tag_15"
  ]
}
```
# Comment
```
POST /users

{
  "user": {
    "email": "author-yqy5fg@email.com",
    "username": "author-yqy5fg",
    "password": "password"
  }
}
```
```
200 OK

{
  "user": {
    "email": "author-yqy5fg@email.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImF1dGhvci15cXk1ZmciLCJpYXQiOjE1OTU4OTQ5ODcsImV4cCI6MTU5NjA2Nzc4N30.6UkOn04Qqkx_9kkz254tnSY8r7C6mtrFhJUP_C16_2I",
    "username": "author-yqy5fg",
    "bio": "",
    "image": ""
  }
}
```
```
POST /users

{
  "user": {
    "email": "commenter-i9cmyq@email.com",
    "username": "commenter-i9cmyq",
    "password": "password"
  }
}
```
```
200 OK

{
  "user": {
    "email": "commenter-i9cmyq@email.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImNvbW1lbnRlci1pOWNteXEiLCJpYXQiOjE1OTU4OTQ5ODcsImV4cCI6MTU5NjA2Nzc4N30.RDgmJqrq3ZnJjPjIydi7f6AtlA8NJQ80BzYOplrhuV4",
    "username": "commenter-i9cmyq",
    "bio": "",
    "image": ""
  }
}
```
```
POST /articles

{
  "article": {
    "title": "title",
    "description": "description",
    "body": "body"
  }
}
```
```
200 OK

{
  "article": {
    "slug": "title-ydv9z",
    "title": "title",
    "description": "description",
    "body": "body",
    "createdAt": 1595894987596,
    "updatedAt": 1595894987596,
    "author": {
      "username": "author-yqy5fg",
      "bio": "",
      "image": "",
      "following": false
    },
    "tagList": [],
    "favorited": false,
    "favoritesCount": 0
  }
}
```
## Create
### should create comment
```
POST /articles/title-ydv9z/comments

{
  "comment": {
    "body": "test comment p4hj9n"
  }
}
```
```
200 OK

{
  "comment": {
    "id": "0ad0dc99-40f1-4541-b63f-19006440ee37",
    "slug": "title-ydv9z",
    "body": "test comment p4hj9n",
    "createdAt": 1595894987626,
    "updatedAt": 1595894987626,
    "author": {
      "username": "commenter-i9cmyq",
      "bio": "",
      "image": "",
      "following": false
    }
  }
}
```
```
POST /articles/title-ydv9z/comments

{
  "comment": {
    "body": "test comment 4bjn01"
  }
}
```
```
200 OK

{
  "comment": {
    "id": "dd8e0da6-7811-4b38-b45e-5535b99e6d0e",
    "slug": "title-ydv9z",
    "body": "test comment 4bjn01",
    "createdAt": 1595894987656,
    "updatedAt": 1595894987656,
    "author": {
      "username": "commenter-i9cmyq",
      "bio": "",
      "image": "",
      "following": false
    }
  }
}
```
```
POST /articles/title-ydv9z/comments

{
  "comment": {
    "body": "test comment 19hvmr"
  }
}
```
```
200 OK

{
  "comment": {
    "id": "127d4626-3bdb-4996-b5fb-6d92e667ea03",
    "slug": "title-ydv9z",
    "body": "test comment 19hvmr",
    "createdAt": 1595894987707,
    "updatedAt": 1595894987707,
    "author": {
      "username": "commenter-i9cmyq",
      "bio": "",
      "image": "",
      "following": false
    }
  }
}
```
```
POST /articles/title-ydv9z/comments

{
  "comment": {
    "body": "test comment yfauhy"
  }
}
```
```
200 OK

{
  "comment": {
    "id": "b2373d85-4351-41a9-a63e-9bfcc575a578",
    "slug": "title-ydv9z",
    "body": "test comment yfauhy",
    "createdAt": 1595894987743,
    "updatedAt": 1595894987743,
    "author": {
      "username": "commenter-i9cmyq",
      "bio": "",
      "image": "",
      "following": false
    }
  }
}
```
```
POST /articles/title-ydv9z/comments

{
  "comment": {
    "body": "test comment z71ka0"
  }
}
```
```
200 OK

{
  "comment": {
    "id": "df42eca1-b95b-4018-bb1d-96ab3c265b23",
    "slug": "title-ydv9z",
    "body": "test comment z71ka0",
    "createdAt": 1595894987774,
    "updatedAt": 1595894987774,
    "author": {
      "username": "commenter-i9cmyq",
      "bio": "",
      "image": "",
      "following": false
    }
  }
}
```
```
POST /articles/title-ydv9z/comments

{
  "comment": {
    "body": "test comment ucpz6"
  }
}
```
```
200 OK

{
  "comment": {
    "id": "85f840a5-f538-4511-b175-e4709da2e693",
    "slug": "title-ydv9z",
    "body": "test comment ucpz6",
    "createdAt": 1595894987807,
    "updatedAt": 1595894987807,
    "author": {
      "username": "commenter-i9cmyq",
      "bio": "",
      "image": "",
      "following": false
    }
  }
}
```
```
POST /articles/title-ydv9z/comments

{
  "comment": {
    "body": "test comment dif44z"
  }
}
```
```
200 OK

{
  "comment": {
    "id": "5ff53dea-78fd-4437-b2c8-a9f83933db13",
    "slug": "title-ydv9z",
    "body": "test comment dif44z",
    "createdAt": 1595894987839,
    "updatedAt": 1595894987839,
    "author": {
      "username": "commenter-i9cmyq",
      "bio": "",
      "image": "",
      "following": false
    }
  }
}
```
```
POST /articles/title-ydv9z/comments

{
  "comment": {
    "body": "test comment kmkufw"
  }
}
```
```
200 OK

{
  "comment": {
    "id": "06c59f46-718c-4fd5-8294-56a38cd127c5",
    "slug": "title-ydv9z",
    "body": "test comment kmkufw",
    "createdAt": 1595894987868,
    "updatedAt": 1595894987868,
    "author": {
      "username": "commenter-i9cmyq",
      "bio": "",
      "image": "",
      "following": false
    }
  }
}
```
```
POST /articles/title-ydv9z/comments

{
  "comment": {
    "body": "test comment r3ikkt"
  }
}
```
```
200 OK

{
  "comment": {
    "id": "fc09fd94-0703-44d0-aa93-2463873e2493",
    "slug": "title-ydv9z",
    "body": "test comment r3ikkt",
    "createdAt": 1595894987905,
    "updatedAt": 1595894987905,
    "author": {
      "username": "commenter-i9cmyq",
      "bio": "",
      "image": "",
      "following": false
    }
  }
}
```
```
POST /articles/title-ydv9z/comments

{
  "comment": {
    "body": "test comment -z65m7n"
  }
}
```
```
200 OK

{
  "comment": {
    "id": "7e040e7b-2ba1-4ec9-a04d-c6f0b19130d3",
    "slug": "title-ydv9z",
    "body": "test comment -z65m7n",
    "createdAt": 1595894987944,
    "updatedAt": 1595894987944,
    "author": {
      "username": "commenter-i9cmyq",
      "bio": "",
      "image": "",
      "following": false
    }
  }
}
```
### should disallow unauthenticated user
```
POST /articles/title-ydv9z/comments

{}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Must be logged in."
    ]
  }
}
```
### should enforce comment body
```
POST /articles/title-ydv9z/comments

{}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Comment must be specified."
    ]
  }
}
```
### should disallow non-existent article
```
POST /articles/foobar/comments

{
  "comment": {
    "body": "test comment 3envxj"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Article not found: [foobar]"
    ]
  }
}
```
## Get
### should get all comments for article
```
GET /articles/title-ydv9z/comments
```
```
200 OK

{
  "comments": [
    {
      "createdAt": 1595894987905,
      "id": "fc09fd94-0703-44d0-aa93-2463873e2493",
      "body": "test comment r3ikkt",
      "slug": "title-ydv9z",
      "author": {
        "username": "commenter-i9cmyq",
        "bio": "",
        "image": "",
        "following": false
      },
      "updatedAt": 1595894987905
    },
    {
      "createdAt": 1595894987626,
      "id": "0ad0dc99-40f1-4541-b63f-19006440ee37",
      "body": "test comment p4hj9n",
      "slug": "title-ydv9z",
      "author": {
        "username": "commenter-i9cmyq",
        "bio": "",
        "image": "",
        "following": false
      },
      "updatedAt": 1595894987626
    },
    {
      "createdAt": 1595894987743,
      "id": "b2373d85-4351-41a9-a63e-9bfcc575a578",
      "body": "test comment yfauhy",
      "slug": "title-ydv9z",
      "author": {
        "username": "commenter-i9cmyq",
        "bio": "",
        "image": "",
        "following": false
      },
      "updatedAt": 1595894987743
    },
    {
      "createdAt": 1595894987774,
      "id": "df42eca1-b95b-4018-bb1d-96ab3c265b23",
      "body": "test comment z71ka0",
      "slug": "title-ydv9z",
      "author": {
        "username": "commenter-i9cmyq",
        "bio": "",
        "image": "",
        "following": false
      },
      "updatedAt": 1595894987774
    },
    {
      "createdAt": 1595894987707,
      "id": "127d4626-3bdb-4996-b5fb-6d92e667ea03",
      "body": "test comment 19hvmr",
      "slug": "title-ydv9z",
      "author": {
        "username": "commenter-i9cmyq",
        "bio": "",
        "image": "",
        "following": false
      },
      "updatedAt": 1595894987707
    },
    {
      "createdAt": 1595894987839,
      "id": "5ff53dea-78fd-4437-b2c8-a9f83933db13",
      "body": "test comment dif44z",
      "slug": "title-ydv9z",
      "author": {
        "username": "commenter-i9cmyq",
        "bio": "",
        "image": "",
        "following": false
      },
      "updatedAt": 1595894987839
    },
    {
      "createdAt": 1595894987868,
      "id": "06c59f46-718c-4fd5-8294-56a38cd127c5",
      "body": "test comment kmkufw",
      "slug": "title-ydv9z",
      "author": {
        "username": "commenter-i9cmyq",
        "bio": "",
        "image": "",
        "following": false
      },
      "updatedAt": 1595894987868
    },
    {
      "createdAt": 1595894987656,
      "id": "dd8e0da6-7811-4b38-b45e-5535b99e6d0e",
      "body": "test comment 4bjn01",
      "slug": "title-ydv9z",
      "author": {
        "username": "commenter-i9cmyq",
        "bio": "",
        "image": "",
        "following": false
      },
      "updatedAt": 1595894987656
    },
    {
      "createdAt": 1595894987807,
      "id": "85f840a5-f538-4511-b175-e4709da2e693",
      "body": "test comment ucpz6",
      "slug": "title-ydv9z",
      "author": {
        "username": "commenter-i9cmyq",
        "bio": "",
        "image": "",
        "following": false
      },
      "updatedAt": 1595894987807
    },
    {
      "createdAt": 1595894987944,
      "id": "7e040e7b-2ba1-4ec9-a04d-c6f0b19130d3",
      "body": "test comment -z65m7n",
      "slug": "title-ydv9z",
      "author": {
        "username": "commenter-i9cmyq",
        "bio": "",
        "image": "",
        "following": false
      },
      "updatedAt": 1595894987944
    }
  ]
}
```
## Delete
### should delete comment
```
DELETE /articles/title-ydv9z/comments/0ad0dc99-40f1-4541-b63f-19006440ee37
```
```
200 OK

{}
```
### only comment author should be able to delete comment
```
DELETE /articles/title-ydv9z/comments/dd8e0da6-7811-4b38-b45e-5535b99e6d0e
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Only comment author can delete: [commenter-i9cmyq]"
    ]
  }
}
```
### should disallow unauthenticated user
```
DELETE /articles/title-ydv9z/comments/dd8e0da6-7811-4b38-b45e-5535b99e6d0e
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Must be logged in."
    ]
  }
}
```
### should disallow deleting unknown comment
```
DELETE /articles/title-ydv9z/comments/foobar_id
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Comment ID not found: [foobar_id]"
    ]
  }
}
```
# User
## Create
### should create user
```
POST /users

{
  "user": {
    "email": "user1-0.3xvo3aizoiy@email.com",
    "username": "user1-0.3xvo3aizoiy",
    "password": "password"
  }
}
```
```
200 OK

{
  "user": {
    "email": "user1-0.3xvo3aizoiy@email.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InVzZXIxLTAuM3h2bzNhaXpvaXkiLCJpYXQiOjE1OTU4OTQ5ODgsImV4cCI6MTU5NjA2Nzc4OH0.F-ph8EP8AVGD6kksBqRzLUYU92wlo3gOL_wzJnHEwEw",
    "username": "user1-0.3xvo3aizoiy",
    "bio": "",
    "image": ""
  }
}
```
### should disallow same username
```
POST /users

{
  "user": {
    "email": "user1-0.3xvo3aizoiy@email.com",
    "username": "user1-0.3xvo3aizoiy",
    "password": "password"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Username already taken: [user1-0.3xvo3aizoiy]"
    ]
  }
}
```
### should disallow same email
```
POST /users

{
  "user": {
    "username": "user2",
    "email": "user1-0.3xvo3aizoiy@email.com",
    "password": "password"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Email already taken: [user1-0.3xvo3aizoiy@email.com]"
    ]
  }
}
```
### should enforce required fields
```
POST /users

{}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "User must be specified."
    ]
  }
}
```
```
POST /users

{
  "user": {
    "foo": 1
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Username must be specified."
    ]
  }
}
```
```
POST /users

{
  "user": {
    "username": 1
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Email must be specified."
    ]
  }
}
```
```
POST /users

{
  "user": {
    "username": 1,
    "email": 2
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Password must be specified."
    ]
  }
}
```
## Login
### should login
```
POST /users/login

{
  "user": {
    "email": "user1-0.3xvo3aizoiy@email.com",
    "password": "password"
  }
}
```
```
200 OK

{
  "user": {
    "email": "user1-0.3xvo3aizoiy@email.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InVzZXIxLTAuM3h2bzNhaXpvaXkiLCJpYXQiOjE1OTU4OTQ5ODgsImV4cCI6MTU5NjA2Nzc4OH0.F-ph8EP8AVGD6kksBqRzLUYU92wlo3gOL_wzJnHEwEw",
    "username": "user1-0.3xvo3aizoiy",
    "bio": "",
    "image": ""
  }
}
```
### should disallow unknown email
```
POST /users/login

{
  "user": {
    "email": "0.q34w8hih61",
    "password": "somepassword"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Email not found: [0.q34w8hih61]"
    ]
  }
}
```
### should disallow wrong password
```
POST /users/login

{
  "user": {
    "email": "user1-0.3xvo3aizoiy@email.com",
    "password": "0.tll233qcfps"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Wrong password."
    ]
  }
}
```
### should enforce required fields
```
POST /users/login

{}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "User must be specified."
    ]
  }
}
```
```
POST /users/login

{
  "user": {}
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Email must be specified."
    ]
  }
}
```
```
POST /users/login

{
  "user": {
    "email": "someemail"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Password must be specified."
    ]
  }
}
```
## Get
### should get current user
```
GET /user
```
```
200 OK

{
  "user": {
    "email": "user1-0.3xvo3aizoiy@email.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InVzZXIxLTAuM3h2bzNhaXpvaXkiLCJpYXQiOjE1OTU4OTQ5ODgsImV4cCI6MTU5NjA2Nzc4OH0.F-ph8EP8AVGD6kksBqRzLUYU92wlo3gOL_wzJnHEwEw",
    "username": "user1-0.3xvo3aizoiy",
    "bio": "",
    "image": ""
  }
}
```
### should disallow bad tokens
```
GET /user
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Token not present or invalid."
    ]
  }
}
```
```
GET /user
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Token not present or invalid."
    ]
  }
}
```
```
GET /user
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Token not present or invalid."
    ]
  }
}
```
```
GET /user
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Token not present or invalid."
    ]
  }
}
```
## Profile
### should get profile
```
GET /profiles/user1-0.3xvo3aizoiy
```
```
200 OK

{
  "profile": {
    "username": "user1-0.3xvo3aizoiy",
    "bio": "",
    "image": "",
    "following": false
  }
}
```
### should disallow unknown username
```
GET /profiles/foo_0.o9kqvxofun
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "User not found: [foo_0.o9kqvxofun]"
    ]
  }
}
```
### should follow/unfollow user
```
POST /users

{
  "user": {
    "username": "followed_user",
    "email": "followed_user@mail.com",
    "password": "password"
  }
}
```
```
200 OK

{
  "user": {
    "email": "followed_user@mail.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImZvbGxvd2VkX3VzZXIiLCJpYXQiOjE1OTU4OTQ5ODgsImV4cCI6MTU5NjA2Nzc4OH0.A2yd7CNFPIZygS8m9a0qso0qMuGor_lkdsRcWXFAp6I",
    "username": "followed_user",
    "bio": "",
    "image": ""
  }
}
```
```
POST /profiles/followed_user/follow
```
```
200 OK

{
  "profile": {
    "username": "followed_user",
    "bio": "",
    "image": "",
    "following": true
  }
}
```
```
POST /profiles/followed_user/follow
```
```
200 OK

{
  "profile": {
    "username": "followed_user",
    "bio": "",
    "image": "",
    "following": true
  }
}
```
```
GET /profiles/followed_user
```
```
200 OK

{
  "profile": {
    "username": "followed_user",
    "bio": "",
    "image": "",
    "following": true
  }
}
```
```
GET /profiles/followed_user
```
```
200 OK

{
  "profile": {
    "username": "followed_user",
    "bio": "",
    "image": "",
    "following": false
  }
}
```
```
POST /users

{
  "user": {
    "username": "user2-0.rt8qagy8rlo",
    "email": "user2-0.rt8qagy8rlo@mail.com",
    "password": "password"
  }
}
```
```
200 OK

{
  "user": {
    "email": "user2-0.rt8qagy8rlo@mail.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InVzZXIyLTAucnQ4cWFneThybG8iLCJpYXQiOjE1OTU4OTQ5ODgsImV4cCI6MTU5NjA2Nzc4OH0.6H_FXU1yDy_nnSyW2r2f_kHtPxvbga19lNKPq13iA_I",
    "username": "user2-0.rt8qagy8rlo",
    "bio": "",
    "image": ""
  }
}
```
```
POST /profiles/followed_user/follow
```
```
200 OK

{
  "profile": {
    "username": "followed_user",
    "bio": "",
    "image": "",
    "following": true
  }
}
```
```
DELETE /profiles/followed_user/follow
```
```
200 OK

{
  "profile": {
    "username": "followed_user",
    "bio": "",
    "image": "",
    "following": false
  }
}
```
```
DELETE /profiles/followed_user/follow
```
```
200 OK

{
  "profile": {
    "username": "followed_user",
    "bio": "",
    "image": "",
    "following": false
  }
}
```
```
DELETE /profiles/followed_user/follow
```
```
200 OK

{
  "profile": {
    "username": "followed_user",
    "bio": "",
    "image": "",
    "following": false
  }
}
```
### should disallow following with bad token
```
POST /profiles/followed_user/follow
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Token not present or invalid."
    ]
  }
}
```
## Update
### should update user
```
PUT /user

{
  "user": {
    "email": "updated-user1-0.3xvo3aizoiy@email.com"
  }
}
```
```
200 OK

{
  "user": {
    "username": "user1-0.3xvo3aizoiy",
    "email": "updated-user1-0.3xvo3aizoiy@email.com",
    "image": "",
    "bio": "",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InVzZXIxLTAuM3h2bzNhaXpvaXkiLCJpYXQiOjE1OTU4OTQ5ODgsImV4cCI6MTU5NjA2Nzc4OH0.F-ph8EP8AVGD6kksBqRzLUYU92wlo3gOL_wzJnHEwEw"
  }
}
```
```
PUT /user

{
  "user": {
    "password": "newpassword"
  }
}
```
```
200 OK

{
  "user": {
    "username": "user1-0.3xvo3aizoiy",
    "email": "updated-user1-0.3xvo3aizoiy@email.com",
    "image": "",
    "bio": "",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InVzZXIxLTAuM3h2bzNhaXpvaXkiLCJpYXQiOjE1OTU4OTQ5ODgsImV4cCI6MTU5NjA2Nzc4OH0.F-ph8EP8AVGD6kksBqRzLUYU92wlo3gOL_wzJnHEwEw"
  }
}
```
```
PUT /user

{
  "user": {
    "bio": "newbio"
  }
}
```
```
200 OK

{
  "user": {
    "username": "user1-0.3xvo3aizoiy",
    "bio": "newbio",
    "image": "",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InVzZXIxLTAuM3h2bzNhaXpvaXkiLCJpYXQiOjE1OTU4OTQ5ODgsImV4cCI6MTU5NjA2Nzc4OH0.F-ph8EP8AVGD6kksBqRzLUYU92wlo3gOL_wzJnHEwEw"
  }
}
```
```
PUT /user

{
  "user": {
    "image": "newimage"
  }
}
```
```
200 OK

{
  "user": {
    "username": "user1-0.3xvo3aizoiy",
    "image": "newimage",
    "bio": "newbio",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InVzZXIxLTAuM3h2bzNhaXpvaXkiLCJpYXQiOjE1OTU4OTQ5ODgsImV4cCI6MTU5NjA2Nzc4OH0.F-ph8EP8AVGD6kksBqRzLUYU92wlo3gOL_wzJnHEwEw"
  }
}
```
### should disallow missing token/email in update
```
PUT /user
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Token not present or invalid."
    ]
  }
}
```
```
PUT /user

{}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "User must be specified."
    ]
  }
}
```
### should disallow reusing email
```
POST /users

{
  "user": {
    "email": "user2-0.z0jch6e9hn@email.com",
    "username": "user2-0.z0jch6e9hn",
    "password": "password"
  }
}
```
```
200 OK

{
  "user": {
    "email": "user2-0.z0jch6e9hn@email.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InVzZXIyLTAuejBqY2g2ZTlobiIsImlhdCI6MTU5NTg5NDk4OSwiZXhwIjoxNTk2MDY3Nzg5fQ.olec-Mix9CfRcRcivfZ-O4wxv-yBPV6gzGNKmfN0uxc",
    "username": "user2-0.z0jch6e9hn",
    "bio": "",
    "image": ""
  }
}
```
```
PUT /user

{
  "user": {
    "email": "user2-0.z0jch6e9hn@email.com"
  }
}
```
```
422 Unprocessable Entity

{
  "errors": {
    "body": [
      "Email already taken: [user2-0.z0jch6e9hn@email.com]"
    ]
  }
}
```
# Util
## Ping
### should ping
```
GET /ping
```
```
200 OK

{
  "pong": "2020-07-28T00:09:49.050Z",
  "AWS_REGION": "us-east-1",
  "DYNAMODB_NAMESPACE": "dev"
}
```
