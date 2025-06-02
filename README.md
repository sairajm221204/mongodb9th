# mongodb9th
a.	Develop a query to demonstrate Text search using catalog data collection for a given word
Create Sample catalog Collection
db.catalog.insertMany([
  {
    title: "Learn Python Programming",
    description: "This book teaches you the fundamentals of Python programming."
  },
  {
    title: "Mastering JavaScript",
    description: "A deep dive into JavaScript and modern web development."
  },
  {
    title: "Data Science with Python",
    description: "Explore data analysis, visualization, and machine learning using Python."
  },
  {
    title: "Web Development Basics",
    description: "HTML, CSS, and JavaScript for beginners in web development."
  }
]);

Create a Text Index
db.catalog.createIndex({
  title: "text",
  description: "text"
});





Perform a Text Search
db.catalog.find({
  $text: { $search: "Python" }
});

Sample Output
[
  {
    "_id": ObjectId("..."),
    "title": "Learn Python Programming",
    "description": "This book teaches you the fundamentals of Python programming."
  },
  {
    "_id": ObjectId("..."),
    "title": "Data Science with Python",
    "description": "Explore data analysis, visualization, and machine learning using Python."
  }
]

To sort results by relevance, use the textScore:
db.catalog.find(
  { $text: { $search: "Python" } },
  { score: { $meta: "textScore" }, title: 1, description: 1 }
).sort({ score: { $meta: "textScore" } });

 

b.	Develop queries to illustrate excluding documents with certain words and phrases

db.catalog.insertMany([
  { name: "Canon DSLR Camera", description: "High quality digital camera for photography." },
  { name: "Sony Headphones", description: "Noise-canceling wireless headphones." },
  { name: "Apple iPhone", description: "Latest model smartphone with advanced camera features." },
  { name: "Samsung TV", description: "Smart TV with 4K resolution." },
  { name: "Nikon Mirrorless Camera", description: "Compact camera for professional photos." }
]);


Exclude a Certain Word (e.g., Exclude "camera")
db.catalog.find(
  { $text: { $search: "-camera" } }
);

Explanation:
•	The minus sign (-) means NOT.
•	So this query excludes all documents containing "camera".
Expected Results:
•	Only documents like Sony Headphones, Samsung TV will be returned.

Exclude a Word While Including Another (e.g., Find "phone" but not "camera")

db.catalog.find(
  { $text: { $search: "phone -camera" } }
);

Explanation:
•	Find documents containing "phone" but NOT "camera".
Example match:
•	Sony Headphones (if description contains "phone" somehow)
•	Apple iPhone (yes, "phone" but no "camera" in the name — but check description carefully!)


Exclude an Exact Phrase (e.g., Exclude "professional photos")

db.catalog.find(
  { $text: { $search: '-"professional photos"' } }
);
Explanation:
•	Quotes "..." mean exact phrase.
•	Minus - means NOT that phrase.
•	So, skip documents containing exactly "professional photos".

db.catalog.find(
  { $text: { $search: 'camera -"professional photos"' } },
  { score: { $meta: "textScore" } }
).sort({ score: { $meta: "textScore" } });

 



