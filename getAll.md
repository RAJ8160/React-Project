#  GetAll

This guide explains how to **fetch and display posts** from JSONPlaceholder using **React** and **Axios**, with optional **delete functionality**.

## Step-1
* Create axious instance in  src>api>PostApi.jsx

```jsx
import axios from "axios";

const api = axios.create({
    baseURL:"https://jsonplaceholder.typicode.com",
});
---

# Step-2
* Create get method in src>api>PostApi.jsx
---
// get method
export const getPost = () =>{
    return api.get("/posts");
}

# Step-3
* Create Posts.jsx and write in this
```jsx
import React, { useEffect, useState } from 'react'
import { motion } from 'framer-motion';
import { deletePost, getPost } from '../api/PostApi';

const Posts = () => {

    const [data, setData] = useState([]);

     useEffect(() => {
        const fetchData = async () => {
            const res = await MyAPI.getData();
            setData(res);
        };

        fetchData();
    }, [])

 return (
        <div className='container'>
        <div className='row'>
            <AddUser data={data} setData={setData}/>
        </div>
        <div className='m-4  d-flex flex-row flex-wrap gap-3'>
            {data.map((elem, idx) => (
                <div key={elem.id} className="card" style={{ width: "18rem" }}>
                    <div className="card-body">
                        <h2 className="rounded-5 bg-success text-center" style={{ width: "3rem", height: "3rem" }}>{elem.id}</h2>
                        <h5 className="card-title">Title : {elem.title}</h5>
                        <p className="card-text">Body :{elem.body}</p>
                        <motion.button whileHover={{ scale: 1.05, y: -2 }} whileTap={{ scale: 0.9, y: 1 }} transition={{ type: "spring" }} type="button" className="btn btn-outline-success">Success</motion.button>
                        <motion.button whileHover={{ scale: 1.05, y: -2 }} whileTap={{ scale: 0.9, y: 1 }} transition={{ type: "spring" }} type="button" className="btn btn-outline-danger m-2" onClick={() => handleDelete(elem.id)}>Danger</motion.button>
                    </div>
                </div>
            ))}
        </div>
        </div>
    )
}

export default Posts
---