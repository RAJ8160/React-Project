## This is Delete Element From API

# Step-1
* Create delete method From <h1>api</h1>
```jsx
//delete method
export const deletePost = (id) =>{
    return api.delete(`/posts/${id}`);
}
---

# Step-2
* Write function to delete post
```jsx
 //function to delete Post
    const handleDelete = async (id) => {
        try {
            const res = await deletePost(id)
            if (res.status === 200 || res.status === 204) {
                const newData = data.filter(c => {
                    return c.id !== id
                });

                setData(newData)
            }
            console.log(res);
        } catch (error) {
            console.log("Error During Deletion:", error);

        }
    };
---

# Step-3
* add this function into delete button
```jsx
<motion.button whileHover={{ scale: 1.05, y: -2 }} whileTap={{ scale: 0.9, y: 1 }} transition={{ type: "spring" }} type="button" className="btn btn-outline-danger m-2" onClick={() => handleDelete(elem.id)}>Danger</motion.button>
---