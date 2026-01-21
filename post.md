## Step - 1
# Create for component for add-Data
```jsx
 <div className='row'>
        <AddUser data={data} setData={setData}/>
  </div>
---
---
## Step - 2
# Create <AddUser> Componenet
```jsx
import React, { useState } from 'react'

const AddUser = ({data,setData}) => {
    return (
    <div className='container p-5'>
      <form onSubmit={handleFormSubmit}>
        <div className='row'>
          <div className="mb-3 col-md-6">
            <label
              htmlFor="exampleInputEmail1"
              className="form-label"
            >
              Title
            </label>
            <input
              type="text"
              className="form-control"
              id="exampleInputEmail1"
              aria-describedby="emailHelp"
              name='title'
              value={addData.title}          // This is for value
              onChange={handleInputChange}   // We fire function on OnChange
            />
            {/* <div id="emailHelp" className="form-text">We'll never share your email with anyone else.</div> */}
          </div>
          <div className="mb-3 col-md-6">
            <label
              htmlFor="exampleInputPassword1"
              className="form-label"
            >
              Body Content
            </label>
            <input
              type="text"
              className="form-control"
              id="exampleInputPassword1"
              name='body'
              value={addData.body}           // This is for value
              onChange={handleInputChange}   // We fire function on OnChange
            />
          </div>
          <div className="mb-3 form-check">
            <input
              type="checkbox"
              className="form-check-input"
              id="exampleCheck1"
            />
            <label
              className="form-check-label"
              htmlFor="exampleCheck1"
            >
              Check me out
            </label>
          </div>
          <button type="submit" className="btn btn-primary">Submit</button>
        </div>
      </form>
    </div>
  )
}

export default AddUser
---
## Step - 3 
# Create post method in <h1>api</h1> folder
```jsx
//post method
export const postData = (post) =>{
    //This is Our convention we pass payload with route
    return api.post("/posts",post);
}
---

## Step -4
# Create useSate in AddUser.jsx
```jsx
const [addData, setAddData] = useState({
    title: "",
    body: ""
  })
---

## Step - 5
# Create handleInputeChange function
```jsx
 const handleInputChange = (e) => {
    const name = e.target.name;
    const value = e.target.value;

    setAddData((prev) => {
      return {
        ...prev, [name]: value
      }
    })
  }
---
## Step - 6
# Create handleSubmitForm function

```jsx
 const handleFormSubmit = (e) => {
    e.preventDefault();
    addPostData();
  }
---

## Step - 7
# addPostData arrow function make it asynchronus becuase we want some time to add Data in previous data
```jsx
 const addPostData = async () => {
    try {
      const res = await postData(addData)
      console.log("res ", res);

      if ((res.status === 201)) {
        const nextId = data.length>0?Math.max(...data.map(item => item.id)) + 1:1;

        const newData = {
          ...res.data,
          id:nextId
        }
        setData([...data,newData])
        setAddData({ title: "", body: "" })
      }
    } catch (error) {
      console.error("Error posting data:", error)
    }
  }

---
