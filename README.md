# mongodb-query

### 1. Membuat database `academic`

input:

`use academic`

output:

```
> use academic
switched to db academic
```

### 2. Menampilkan semua collection dan data dalam database

input:

`show collections`

output:

```
> show collections
>
```

### 3. Membuat atau mengaktifkan database

input:

`use academic`

output:

```
> use academic
switched to db academic
```


### 4. Membuat collection `departemen`.

input:

`> db.createCollection("department")`

output:

```
{ "ok" : 1 }
```

### 5. Membuat collection `student`.

input:

`> db.createCollection("student")`

output:

```
{ "ok" : 1 }
```

### 6. Melihat struktur collection `student`.

input:
```
> var schema = db.student.findOne()
> for(var key in schema){print (key, typeof key);}
```

output:
```
_id string
nim string
name string
address string
department string
```


### 7. Menginputkan 5 data ke dalam collection `departemen`.

```
db.department.insert([
  {
    code: "FTI",
    name: "Fakultas Teknik Informatika",
    major: [{name: "Sistem Informasi"},{name: "Teknik Informatika"}]
  },
  {
    code: "FEK",
    name: "Fakultas Ekonomi dan Komunikasi",
    major:[{name: "Akuntansi"}, {name: "Keuangan"}, {name: "Komunikasi Pemasaran"}]
  },
  {
    code: "SD",
    name: "Sekolah Design",
    major: [{name: "Desain Interior"},{name: "Desain Komunikasi Visual"},{name: "Desain Komunikasi Visual Media Baru"}]
  },
  {
    code: "SBM",
    name: "Sekolah Bisnis dan Manajemen",
    major:[{name: "Bisnis & Manajemen Internasional"}, {name: "Manajeman"}, {name: "Pemasaran Internasional"}]
  },
  {
    code: "FT",
    name: "Fakultas Teknik",
    major: [{name: "Arsitektur"},{name: "Teknik Industri"},{name: "Teknik Komputer"}]
  }]
  );
```
output:

```
BulkWriteResult({
       	"writeErrors" : [ ],
       	"writeConcernErrors" : [ ],
       	"nInserted" : 5,
       	"nUpserted" : 0,
       	"nMatched" : 0,
       	"nModified" : 0,
       	"nRemoved" : 0,
       	"upserted" : [ ]
})
```

### 8. Menginputkan 3 data ke dalam collection `student` beserta reference ke `departemen`.

input:
```
db.student.insert([
  {
    nim: "21212018",
    name: "Didit Suryadi",
    address: "Jalan Surya Kencana, Bogor",
    department: {
      $ref : "department",
      $id : ObjectId("58901756ca49c7522b60680f"),
      $db : "academic"
    }
  },
  {
    nim: "2001600595",
    name: "Didit Suryadi",
    address: "Jalan kemanggisan ilir 3",
    department: {
      $ref : "department",
      $id : ObjectId("58901756ca49c7522b60680b"),
      $db : "academic"
    }
  },
  {
    nim: "40484956",
    name: "Arisandika",
    address: "Bintaro",
    department: {
      $ref : "department",
      $id : ObjectId("58901756ca49c7522b60680e"),
      $db : "academic"
    }
  },
  ]);
```
output:

```
BulkWriteResult({
       	"writeErrors" : [ ],
       	"writeConcernErrors" : [ ],
       	"nInserted" : 3,
       	"nUpserted" : 0,
       	"nMatched" : 0,
       	"nModified" : 0,
       	"nRemoved" : 0,
       	"upserted" : [ ]
})
```

### 9. Melihat isi collection `student`.

input:
`db.student.find()`

output:
```
{ "_id" : ObjectId("58901a80ca49c7522b606810"), "nim" : "21212018", "name" : "Didit Suryadi", "address" : "Jalan Surya Kencana, Bogor", "department" : DBRef("department", ObjectId("58901756ca49c7522b60680f"), "academic") }
{ "_id" : ObjectId("58901a80ca49c7522b606811"), "nim" : "2001600595", "name" : "Didit Suryadi", "address" : "Jalan kemanggisan ilir 3", "department" : DBRef("department", ObjectId("58901756ca49c7522b60680b"), "academic") }
{ "_id" : ObjectId("58901a80ca49c7522b606812"), "nim" : "40484956", "name" : "Arisandika", "address" : "Bintaro", "department" : DBRef("department", ObjectId("58901756ca49c7522b60680e"), "academic") }
```

### 10. Menampilkan key `name` dan `address` dalam collection ```student```.

input:
```
db.student.find({},{"name": 1,"address": 1}).pretty();
```

output:
```
{
       	"_id" : ObjectId("58901a80ca49c7522b606810"),
       	"name" : "Didit Suryadi",
       	"address" : "Jalan Surya Kencana, Bogor"
}
{
       	"_id" : ObjectId("58901a80ca49c7522b606811"),
       	"name" : "Didit Suryadi",
       	"address" : "Jalan kemanggisan ilir 3"
}
{
       	"_id" : ObjectId("58901a80ca49c7522b606812"),
       	"name" : "Arisandika",
       	"address" : "Bintaro"
}
```



### 11. Menampilkan key ```StudentId```, ```name```, dan ```address``` dari data student yang mempunyai ```studentId``` tertentu.

input:
```
db.student.find({nim:"21212018"},{"nim": 1,"name": 1,"address":1}).pretty();
```

output:
```
{
       	"_id" : ObjectId("58901a80ca49c7522b606810"),
       	"nim" : "21212018",
       	"name" : "Didit Suryadi",
       	"address" : "Jalan Surya Kencana, Bogor"
}
```

### 12. Menampilkan semua data ```student``` secara urut berdasarkan ```name``` dengan ```sort()``` secara ascending maupun descending.

input:
```
db.student.find().sort({name:1});
db.student.find().sort({name:-1});
```

output:
```
{ "_id" : ObjectId("58901a80ca49c7522b606812"), "nim" : "40484956", "name" : "Arisandika", "address" : "Bintaro", "department" : DBRef("department", ObjectId("58901756ca49c7522b60680e"), "academic") }
{ "_id" : ObjectId("58901a80ca49c7522b606810"), "nim" : "21212018", "name" : "Didit Suryadi", "address" : "Jalan Surya Kencana, Bogor", "department" : DBRef("department", ObjectId("58901756ca49c7522b60680f"), "academic") }
{ "_id" : ObjectId("58901a80ca49c7522b606811"), "nim" : "2001600595", "name" : "Didit Suryadi", "address" : "Jalan kemanggisan ilir 3", "department" : DBRef("department", ObjectId("58901756ca49c7522b60680b"), "academic") }
```
```
{ "_id" : ObjectId("58901a80ca49c7522b606810"), "nim" : "21212018", "name" : "Didit Suryadi", "address" : "Jalan Surya Kencana, Bogor", "department" : DBRef("department", ObjectId("58901756ca49c7522b60680f"), "academic") }
{ "_id" : ObjectId("58901a80ca49c7522b606811"), "nim" : "2001600595", "name" : "Didit Suryadi", "address" : "Jalan kemanggisan ilir 3", "department" : DBRef("department", ObjectId("58901756ca49c7522b60680b"), "academic") }
{ "_id" : ObjectId("58901a80ca49c7522b606812"), "nim" : "40484956", "name" : "Arisandika", "address" : "Bintaro", "department" : DBRef("department", ObjectId("58901756ca49c7522b60680e"), "academic") }
```

### 13. Menampilkan semua data ```department``` secara urut berdasarkan ```name``` secara ascending maupun descending.

input:
```
db.department.find().sort({name:1}).pretty();
db.department.find().sort({name:-1}).pretty();
```

output:
```
{
       	"_id" : ObjectId("58901756ca49c7522b60680c"),
       	"code" : "FEK",
       	"name" : "Fakultas Ekonomi dan Komunikasi",
       	"major" : [
       		{
       			"name" : "Akuntansi"
       		},
       		{
       			"name" : "Keuangan"
       		},
       		{
       			"name" : "Komunikasi Pemasaran"
       		}
       	]
}
{
       	"_id" : ObjectId("58901756ca49c7522b60680f"),
       	"code" : "FT",
       	"name" : "Fakultas Teknik",
       	"major" : [
       		{
       			"name" : "Arsitektur"
       		},
       		{
       			"name" : "Teknik Industri"
       		},
       		{
       			"name" : "Teknik Komputer"
       		}
       	]
}
{
       	"_id" : ObjectId("58901756ca49c7522b60680b"),
       	"code" : "FTI",
       	"name" : "Fakultas Teknik Informatika",
       	"major" : [
       		{
       			"name" : "Sistem Informasi"
       		},
       		{
       			"name" : "Teknik Informatika"
       		}
       	]
}
{
       	"_id" : ObjectId("58901756ca49c7522b60680e"),
       	"code" : "SBM",
       	"name" : "Sekolah Bisnis dan Manajemen",
       	"major" : [
       		{
       			"name" : "Bisnis & Manajemen Internasional"
       		},
       		{
       			"name" : "Manajeman"
       		},
       		{
       			"name" : "Pemasaran Internasional"
       		}
       	]
}
{
       	"_id" : ObjectId("58901756ca49c7522b60680d"),
       	"code" : "SD",
       	"name" : "Sekolah Design",
       	"major" : [
       		{
       			"name" : "Desain Interior"
       		},
       		{
       			"name" : "Desain Komunikasi Visual"
       		},
       		{
       			"name" : "Desain Komunikasi Visual Media Baru"
       		}
       	]
}
```
```
{
       	"_id" : ObjectId("58901756ca49c7522b60680d"),
       	"code" : "SD",
       	"name" : "Sekolah Design",
       	"major" : [
       		{
       			"name" : "Desain Interior"
       		},
       		{
       			"name" : "Desain Komunikasi Visual"
       		},
       		{
       			"name" : "Desain Komunikasi Visual Media Baru"
       		}
       	]
}
{
       	"_id" : ObjectId("58901756ca49c7522b60680e"),
       	"code" : "SBM",
       	"name" : "Sekolah Bisnis dan Manajemen",
       	"major" : [
       		{
       			"name" : "Bisnis & Manajemen Internasional"
       		},
       		{
       			"name" : "Manajeman"
       		},
       		{
       			"name" : "Pemasaran Internasional"
       		}
       	]
}
{
       	"_id" : ObjectId("58901756ca49c7522b60680b"),
       	"code" : "FTI",
       	"name" : "Fakultas Teknik Informatika",
       	"major" : [
       		{
       			"name" : "Sistem Informasi"
       		},
       		{
       			"name" : "Teknik Informatika"
       		}
       	]
}
{
       	"_id" : ObjectId("58901756ca49c7522b60680f"),
       	"code" : "FT",
       	"name" : "Fakultas Teknik",
       	"major" : [
       		{
       			"name" : "Arsitektur"
       		},
       		{
       			"name" : "Teknik Industri"
       		},
       		{
       			"name" : "Teknik Komputer"
       		}
       	]
}
{
       	"_id" : ObjectId("58901756ca49c7522b60680c"),
       	"code" : "FEK",
       	"name" : "Fakultas Ekonomi dan Komunikasi",
       	"major" : [
       		{
       			"name" : "Akuntansi"
       		},
       		{
       			"name" : "Keuangan"
       		},
       		{
       			"name" : "Komunikasi Pemasaran"
       		}
       	]
}
```

### 14. Mencari data ```student``` dengan ```name```.

input:
`db.student.find({name:"Didit Suryadi"}).pretty()`

output:
```
{
       	"_id" : ObjectId("58901a80ca49c7522b606810"),
       	"nim" : "21212018",
       	"name" : "Didit Suryadi",
       	"address" : "Jalan Surya Kencana, Bogor",
       	"department" : DBRef("department", ObjectId("58901756ca49c7522b60680f"), "academic")
}
{
       	"_id" : ObjectId("58901a80ca49c7522b606811"),
       	"nim" : "2001600595",
       	"name" : "Didit Suryadi",
       	"address" : "Jalan kemanggisan ilir 3",
       	"department" : DBRef("department", ObjectId("58901756ca49c7522b60680b"), "academic")
}
```
