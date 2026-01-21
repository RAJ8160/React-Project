## Update Data 

# Step :- 1
* Create handleUpdatePost function in Posts.jsx
```jsx
  <motion.button whileHover={{ scale: 1.05, y: -2 }} whileTap={{ scale: 0.9, y: 1 }} transition={{ type: "spring" }} type="button" className="btn btn-outline-success" onClick={()=>handleUpdatePost(elem)}>Edit</motion.button>
---

# Step :- 2
* Create another State for handle data for Updation in file where you perform getAll
```jsx
const [updateDataApi,setUpdateDataApi] = useState({})
---
# Step :- 3
* Create function to handle update api
```jsx
 //function to handle update data
    const handleUpdatePost = (currElem) =>{
            setUpdateDataApi(currElem)
    }
---

# Step :- 4
* Pass Data as Parameter
 
```jsx
<AddUser data={data} setData={setData} updateDataApi={updateDataApi} setUpdateDataApi={setUpdateDataApi} />
---

# Step :- 4
* in src>components>AddUser
```jsx
const AddUser = ({data,setData ,updateDataApi,setUpdateDataApi}) => {
  const [addData, setAddData] = useState({
    title: "",
    body: ""
  })

//get the updated Data and add into input field

  useEffect(()=>{
    updateDataApi &&
    setAddData({
      title: updateDataApi.title || "",
      body:updateDataApi.body|| ""
    })
  },[updateDataApi])
---

# Step -5
* changes in button
```jsx
<button type="submit" className="btn btn-primary" value={isEmpty?"Add":"Edit"}>{isEmpty?"Add":"Edit"}</button>
---

# Step - 6
* Create method in src>api>PostApi.jsx
```jsx
//put method
export const updateData = (id,post)=>{
    return api.put(`/posts/${id}`,post);
};
---

# Step :- 7
* Make change in submithandeler
```jsx
const handleFormSubmit = (e) => {
    e.preventDefault();
    const action = e.nativeEvent.submitter.value;
    if(action === 'Add'){
          addPostData();
    }else if(action === 'Edit'){
      updatePostData();
    }
  }
---

# Step :- 8
* Make <b>updatePostData()</b> function
```jsx
 // updatePostData
  const updatePostData = async () =>{
    try{
       const res = await updateData(updateDataApi.id,addData);
      console.log("res ",res);

      setData((prev)=>{
        return prev.map((curElem)=>(
           curElem.id === res.data.id?res.data:curElem
        ))
      })
       setAddData({ title: "", body: "" });
       setUpdateDataApi({})
    }catch(error){
      console.log('Error :',error);  
    }  
  }
---
