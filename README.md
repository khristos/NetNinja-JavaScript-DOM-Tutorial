* Navigating the DOM and the Query Selector
* Changing the DOM/manipulating HTML elements, content and CSS
* Interacting with forms and events (e.g. submit, click, keypress)
* Creating new elements via JavaScript and inserting them into the DOM
* Creating a mini reading list app
  https://developer.mozilla.org/en-US/docs/Web/API/Node

  https://developer.mozilla.org/en-US/docs/Web/API/EventTarget

  https://developer.mozilla.org/en-US/docs/Web/API/Element/closest

  https://www.youtube.com/watch?v=FIORjGvT0kk&list=PL4cUxeGkcC9gfoKa5la9dsdCNpuey2s-V

## Temporary Web Server
npx http-server --cors


**LESSON 2 - getElementById**
```javascript
var banner = document.getElementById('page-banner');
const search = document.getElementById('search-books');
const bookList = document.getElementById('book-list');
console.log(search, bookList);
```


**LESSON 3 - [getElementsByClassName, getElementsByTagName (HTMLCollection)]**
```javascript
const titlesByClass = document.getElementsByClassName('title');
const titlesByTag = document.getElementsByTagName('li');
console.log(Array.isArray(titlesByClass));
console.log(Array.isArray(Array.from(titlesByClass)));
console.log(titlesByClass[0]); console.log(titlesByTag[1]);

Array.from(titlesByClass).forEach(function(title){
  console.log(title);
});

for (var i = 0, length = titlesByClass.length;  i < length; i++) {
  console.log(titles[i]);
}
```

**LESSON 4 - querySelector, querySelectorAll [NodeList]**
```javascript
const wmf = document.querySelector('#book-list li:nth-child(2) .name');
console.log(wmf);

var books = document.querySelector('#book-list li .name');
console.log(books);

books = document.querySelectorAll('#book-list li .name');
console.log(books);

Array.from(books).forEach(function(book){
  console.log(book);
});
```

**LESSON 5 - textContent, innerHTML**
```javascript
var books = document.querySelectorAll('#book-list li .name');
/*Array.from(books).forEach(function(book){
  console.log(book.textContent);
});*/

books.forEach(function(book){
  book.textContent += ' (book title)';
});

const bookList = document.querySelector('#book-list');
//bookList.innerHTML = '<h2>Books and more books...</h2>';
bookList.innerHTML += '<p>This is how you add HTML</p>';
```

**LESSON 6 - [Node.nodeType, Node.nodeName, Node.hasChildNodes()]**
```javascript
const banner = document.querySelector('#page-banner');
console.log('#page-banner node type is: ', banner.nodeType);
console.log('#page-banner node name is: ', banner.nodeName);
console.log('#page-banner has child nodes: ', banner.hasChildNodes());

const clonedBanner = banner.cloneNode(true);
console.log(clonedBanner);
```

**LESSON 7 - Traversing the DOM -- [Node.parentNode, Node.parentElement, Node.childNodes, Node.children]**
```javascript
const bookList = document.querySelector('#book-list');

console.log('the parent node is: ', bookList.parentNode);
console.log('the parent element is: ', bookList.parentElement.parentElement);

//console.log(bookList.childNodes);
console.log('all node children:');
Array.from(bookList.childNodes).forEach(function(node){
  console.log(node);
});
//console.log(bookList.children);
console.log('all element children:');
Array.from(bookList.children).forEach(function(node){
  console.log(node);
});

const titles = document.querySelectorAll('.name');
console.log('Book titles:');
Array.from(titles).forEach(function(title){
  console.log(title.textContent);
});
```

**LESSON 8 - Traversing the DOM -- [Node.nextSibling, Node.nextElementSibling, Node.previousSibling, Node.previousElementSibling]**
```javascript
const bookList = document.querySelector('#book-list');

console.log('#book-list next sibling is: ', bookList.nextSibling);
console.log('#book-list next element sibling is: ', bookList.nextElementSibling);
console.log('#book-list previous sibling is: ', bookList.previousSibling);
console.log('#book-list previous element sibling is: ', bookList.previousElementSibling);

bookList.previousElementSibling.querySelector('p').innerHTML += '<br />Too cool!';
```

**LESSON 9 - Events -- [EventTarget.addEventListener()]**
**Attach event listeners to elements**
```javascript
var h2 = document.querySelector('#book-list h2');
h2.addEventListener('click', function(e){
  console.log(e.target);
  console.log(e);
});

var btns = document.querySelectorAll('#book-list .delete');
Array.from(btns).forEach(function(btn){
  //btn.addEventListener('click', function(e){
  btn.addEventListener('click', (e) => {
    const li = e.target.parentElement;
    li.parentNode.removeChild(li);
  });
});

// prevent default behaviour
const link = document.querySelector('#page-banner');
link.addEventListener('click', function(e){
  e.preventDefault();
  console.log('navigation to', e.target.textContent, 'was prevented');
});
```

**Lesson 10 - Event Bubbling**
```javascript
const list = document.querySelector('#book-list ul');

// delete books
//list.addEventListener('click', function(e){
list.addEventListener('click', (e) => {
  if(e.target.className == 'delete'){
    const li = e.target.parentElement;
    //li.parentNode.removeChild(li);
    list.removeChild(li);
  }
});
```

**Lesson 11 - Interacting with Forms (Submit)**
```javascript
const forms = document.forms;
console.dir(forms);
console.log(forms['add-book']);

Array.from(forms).forEach(function(form){
  console.log(form);
});

// add book-list
const addForm = forms['add-book'];
addForm.addEventListener('submit', function(e){
  e.preventDefault();
  const value = addForm.querySelector('input[type="text"]').value;
  console.log(value);
});
```

##Lesson 12 - Creating Elements 
##Lesson 13 - Styles & Classes 
##Lesson 15 - Checkboxes & Change Events 
##Lesson 16 - Custom Search Filter 
##Lesson 17 - Tabbed Content 
##Lesson 18 - DOMContentLoaded Event 
```javascript
document.addEventListener('DOMContentLoaded', function(){
  const list = document.querySelector('#book-list ul');
  const forms = document.forms;

  // delete books
  list.addEventListener('click', (e) => {
    if(e.target.className == 'delete'){
      const li = e.target.parentElement;
      li.parentNode.removeChild(li);
    }
  });

  // add books
  const addForm = forms['add-book'];
  addForm.addEventListener('submit', function(e){
    e.preventDefault();

    // create elements
    const value = addForm.querySelector('input[type="text"]').value;
    const li = document.createElement('li');
    const bookName = document.createElement('span');
    const deleteBtn = document.createElement('span');

    // add text content
    bookName.textContent = value;
    deleteBtn.textContent = 'delete';

    // add classes
    bookName.classList.add('name');
    //bookName.classList.remove('name');
    deleteBtn.classList.add('delete');

    // append to DOM
    li.appendChild(bookName);
    li.appendChild(deleteBtn);
    list.appendChild(li);
    //list.insertBefore(li, list.querySelector('li:first-child'));
    //li.style.color = "red";
    //li.style.marginTop = "60px";
    //li.className = "test1"; li.className += " test2";
  });

    // hide books
    const hideBox = document.querySelector('#hide');
    hideBox.addEventListener('change', function(e){
      if(hideBox.checked){
        list.style.display = "none";
      } else {
        list.style.display = "initial";
      }
    });

    // filter books
    const searchBar = document.forms['search-books'].querySelector('input');
    //searchBar.addEventListener('keyup', function(e) {
    searchBar.addEventListener('keyup', (e) => {
      const term = e.target.value.toLowerCase();
      const books = list.getElementsByTagName('li');
      //Array.from(books).forEach(function(book) {
      Array.from(books).forEach((book) => {
        const title = book.firstElementChild.textContent;
        if (title.toLowerCase().indexOf(term) != -1) {
          book.style.display = 'block';
        } else {
          book.style.display = 'none';
        }
      });
    });

  // tabbed content
  const tabs = document.querySelector('.tabs');
  const panels = document.querySelectorAll('.panel');
  tabs.addEventListener('click', (e) => {
    if(e.target.tagName == 'LI'){
      const targetPanel = document.querySelector(e.target.dataset.target);
      Array.from(panels).forEach((panel) => {
        if(panel == targetPanel){
          panel.classList.add('active');
        }else{
          panel.classList.remove('active');
        }
      });
    }
  });

})
```

**Lesson 14 - Attributes**
```javascript
> const book = document.querySelector('li:first-child .name');
> book
<- <span class="name">Name of the Wind</span>

> book.getAttribute('class');
<- "name"

> book.setAttribute('class', 'name-2');
> book
<- <span class=​"name-2">​Name of the Wind​</span>​

> book.hasAttribute('class');
<- true

> book.removeAttribute('class');
> book
<- <span>​Name of the Wind​</span>​
```


**Lesson 15 - Checkboxes & Change Events**
```javascript
// hide books
const hideBox = document.querySelector('#hide');
hideBox.addEventListener('change', function(e){
  if (hideBox.checked) {
    list.style.display = "none";
  } else {
    list.style.display = "initial";
  }
});
```

**Lesson 16 - Custom Search Filter**
