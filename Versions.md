## Use Transition
Previously : 
```js
function UpdateName({}) {  
const [name, setName] = useState("");  
const [error, setError] = useState(null);  
const [isPending, setIsPending] = useState(false);  

const handleSubmit = async () => {  
	setIsPending(true);  
	const error = await updateName(name);  
	setIsPending(false);  
	if (error) {  
	setError(error);  
	return;  
}  
redirect("/path");  
};
```

Now : 
```js
function UpdateName({}) {  
const [name, setName] = useState("");  
const [error, setError] = useState(null);  
const [isPending, startTransition] = useTransition();  

const handleSubmit = () => {  
	startTransition(async () => {  
	const error = await updateName(name);  
	if (error) {  
	setError(error);  
	return;  
}  

redirect("/path");  
})  
};
```

Here using the `useTransition` hook which handles and async function, Sets the isPending value as `true` until the function is executing once the function is completed sets isPending as `false`

- Instead of using another state, React 19 directly provides it.

## Use Optimistic

```js
function ChangeName({currentName, onUpdateName}) {  
	const [optimisticName, setOptimisticName] = useOptimistic(currentName);  
	const submitAction = async formData => {  
	const newName = formData.get("name");  
	setOptimisticName(newName);  
	const updatedName = await updateName(newName);  
	onUpdateName(updatedName);  
};
```

Here the `useOptimistic` hook set's the name first then executes the `updateName` function and sets the `optimisticName` to `currentName`


