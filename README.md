# jsondb
Just a simple JSON database  
### Installation

Install using `go get github.com/nezamy/jsondb`.

### Usage

```go
// a new jsondb driver, providing the directory where it will be writing to,
db, err := jsondb.New("database_name")
if err != nil {
  fmt.Println("Error", err)
}

// Write a collection to the database
blog := Blog{}
if err := db.Write("blog", "helloWorld", blog); err != nil {
  fmt.Println("Error", err)
}

// Read from collection (passing blog by reference)
firstPost := Blog{}
if err := db.Read("blog", "firstPost", &firstPost); err != nil {
  fmt.Println("Error", err)
}

// Read all records from the collection
records, err := db.ReadAll("blog")
if err != nil {
  fmt.Println("Error", err)
}

posts := []Blog{}
for _, f := range records {
  postFound := Blog{}
  if err := json.Unmarshal([]byte(f), &postFound); err != nil {
    fmt.Println("Error", err)
  }
  posts = append(posts, postFound)
}

// Delete a record from the database
if err := db.Delete("blog", "firstPost"); err != nil {
  fmt.Println("Error", err)
}

// Delete collection from the database
if err := db.Delete("blog", ""); err != nil {
  fmt.Println("Error", err)
}

// List all database collections
list := db.List()
for _, file := range list {
    fmt.Println(file)
}
