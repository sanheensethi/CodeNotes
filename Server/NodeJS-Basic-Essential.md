# NodeJS Basic Essential

# Express Generator
- hum logo ko bhtt sara kaam krna pdta hai express setup krne ke liye, is time ko bcha skte hai using express generator : it will make the folder structure for you, also write the basic code for the project
- npm i express-generator -g
- To create new app: express appname --view=ejs
- cd appname
- npm install

# MongoDB Concept and File Structure
- Code DB Setup -> DB Setup in MongoDB
- Code Model -> Collection in MongoDB
- Code Schema -> Documents in Collection (A Row in Table/Collection)

### Steps:
1. install mongodb
2. install mongojs
3. require setup connection
4. make schema
5. create model and export

```js
//db.js
const mongoose = require("mongoose");
moongose.connect("mongodb://127.0.0.1:27017/<dbname>")

const userSchema = moongose.Schema({
    username: String,
    name: String,
    age: Number
});

// moongose.model(<name of model/Collection>, <Schema>);
userModel = moongose.model("users",userSchema)
module.exports = userModel;
```

```js
var express = require("express");
var router = express.Router();
const userModel = require("./db")

router.get("/",(req,res)=>{
    res.render("index");
})

router.get("/create", async (req,res)=>{
    const data = {
        username: "sanheensethi",
        name: "Sanheen Sethi",
        age:25
    }
   const userDataAfterCreated await userModel.create(data); // async call, either use wait, or use then or error
    res.json(userDataAfterCreated);
})

router.get("/allusers",async (req,res)=>{
    let allusers = userModel.find(); // for only one: userModel.findOne({username:"sanheensethi"});
    if(allusers)
        res.json(allusers);
    res.status(404).json({message:"No User Found."});
})

module.export = router;
```

# Understanding Sessions and Cookies

## Sessions
- it is set inside in server (like cookies in browser)
- session mae jo set kr dia dusre route mae check kr skte hai
- npm install express-session

```js
var express = require("express");
var session = require("express-session");
var router = express.Router();
app.use(session(
    {
        resave:false, // agar ek bari session store hai or uskivalue change na hui ho to dobara save na kro, hr browser ke liye uska session save hota hai.
        saveUninitialized: false, // koi data initialize nhi hai to usko save na kro
        secret: "SECRET_TOKEN_TO_CREATE_HASH" // secret jisse hash bnayenge
    }
)); // app.use -> ye code baar baar chlega jb bhi call krenge

router.get("/",(req,res)=>{
    req.session.banned = true;
    res.json({message:"You are Banned."});
});

// session mae jo set kr dia dusre route mae check kr skte hai
router.get("/checksession",(req,res)=>{
    console.log(req.session);
    res.json({message:"Check Server Console."});
});

//destroy session
router.get("/destroysession",(req,res)=>{
    req.session.destroy((err)=>{
        if (err) throw err;
        res.json({message:"Session Destroyed."});
    })
})

module.exports = router;
```

## Cookie

```js
router.get("/",(req,res)=>{
    res.cookie("age",25); // cookie browser mae bhejna hai to hum cookie ko send kr rhe hai browser side then res we are using.
    res.json({message:"Cookie Set."});
});

router.get("/checkcookie",(req,res)=>{
    console.log(req.cookies); // cookie browser se aayegi if set, then we use this req
    res.json({message:"Got Cookie! Check Server Console."});
});

// delete
res.clearCookie("<cookie-name>");
```

## Connect Flash

- flash allow message to send to another route or page
- install connect-flash (npm i connect-flash)
- use connect flash in app.use function
- kisi bhi router mae flash create krna hai
- kisi bhi dusre route pr use chla skte hai.
- express-session jarori hai iske liye
- flash message ka mtlb kisi route mae data bnana and us data ko dusre route mae data use krna

```js
const express = require("express");
const router = express.Router();
const expressSession = require("express-session");
const flash = require("connect-flash")

app.use(expressSession({
    resave:false,
    saveUninitialized: false,
    secret: "SECRET_TOKEN"
}));

app.use(flash()); // koi bhi route chlega to vo app.use pehle chalyega

router.get("/",(req,res)=>{
    res.json({message:"Hello!"});
});

router.get("/login"(req,res)=>{
    // agar login hojaye to go to profile page
    // agar na ho to mujhe is route se kisi or route pr jana hai ex. /error and vha pr error dekhao
    // to ek route ka data dusre route mae dekhana hai.
    // now here connect-flash comes
    req.flash("name",data); // to set data
})

router.get("/error",(req,res)=>{
    // to get flash data
    req.flash("name");
})
```

# Intermediate MongoDB - Filters and Case Sensitive Search

## How to perform Case Sesitive Seach ?
## How to find documents where an array of fields contains all of a set of values ?
## How can i search for documents with a specific date range in Moongose ?
## How i can filter documents based on existence of field in Moongose ?
## How i can filter documents based on a specific field's length in Moongose ?

```js
const mongoose = require("mongoose");
mongoose.connecct("MONGO_URL");

// schema
const userSchema = mongoose.Schema({
    username:String,
    password: String,
    categories: {
        type: Array,
        default: []
    },
    date_created: {
        type: Date,
        default: Date.now()
    },
});

const userModel = mongoose.model("users",userSchema);
module.exports = userModel;
```

```js
router.get("/create",(req,res)=>{
    await userModel.create({
        "username": "Sanheen",
        "password": "TOEKN"
        categories: ["nodejs","python","scripting","react","bash","selenium", "javascript"],
    })
})

// find
let user = await userModel.({username:"Sanheen"});
// agar capiral mae likha ya small mae koi frk nhi hai, case insensitive bnana hai
// hum RegEx ka use krenge
const pattern1 = new RegEx("Sanheen","i");
let user = await userModel({username:pattern1});

const pattern2 = new RegEx("^Sanheen$","i");
let user = await userModel({username:pattern2});

// i want to find users where categories are common
// find on the basis of categories, sare bnde dhundne hai to likhna hai $all
let user = await userModel.find({ categories: {
    $all: ["fashion"]
}});

// search doc with specific date range
var date1 = new Date("2023-10-02");
var date2 = new Date("2023-10-03");
let user = await userModel.find({
    date_created: {
        $gte: date1,
        $lte: date2
    }
});

// filter doc based on specific field length
// specific field length ke andar sare users dhundne hai

let user = await userModel.find({
    $expr: {
        $and: [
            {$gte: [{$strLenCP: '$username'}], 0},
            {$lte: [{$strLenCP: '$username'}], 12} // 0 to 12 username filed length ke users dega
        ]
    }
})
```




















