# jsondb
Just a simple JSON database  
### Installation

Install using `go get github.com/nezamy/jsondb`.

### Usage

```go
// a new jsondb driver, providing the directory where it will be writing to,
// and a qualified logger if desired
db, err := jsondb.New(dir, nil)
if err != nil {
  fmt.Println("Error", err)
}

// Write a blog to the database
blog := Blog{}
if err := db.Write("blog", "helloWorld", blog); err != nil {
  fmt.Println("Error", err)
}

// Read a blog from the database (passing blog by reference)
firstPost := Blog{}
if err := db.Read("blog", "firstPost", &firstPost); err != nil {
  fmt.Println("Error", err)
}

// Read all fish from the database, unmarshaling the response.
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

// Delete a blog from the database
if err := db.Delete("blog", "firstPost"); err != nil {
  fmt.Println("Error", err)
}

// Delete all blog from the database
if err := db.Delete("blog", ""); err != nil {
  fmt.Println("Error", err)
}
