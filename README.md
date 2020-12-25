
# Anonymous Arrow Functions in React useState

##Example 1: displayUser


 in this example, we're not setting the currentUser with useReducer, so what happens when we edit the account? a refresh is needed in order to display the new info.
This could be solved with useReducer, however, another solution is to set a "display user" to an annonymous function state, that way the new current user information will show after editing 
without reloading the page and without using useReducer.

-In this example, we are finding the user from the allUsers array (get request for all users) 
- if the user's id is equal to the currentUser id, set it to the displayUsers' ID.
- This is an example of why sometimes you'd want to use an arrow function to set a state.

```
  const [allUsers, setAllUsers] = useState([]);

  const [displayUser, setDisplayUser] = useState(() => {
  return allUsers?.find((user) => user?.id === currentUser?.id);
  });

```

Then in the useEffect, we use the displayUser here:

```
useEffect(() => {
    const fetchUsers = async () => {
      const userData = await getAllUsers();
      setAllUsers(userData);
      setDisplayUser(() =>
        userData?.find((user) => user?.id === currentUser?.id)
      );
    };
    fetchUsers();
  }, []);
  }, [currentUser]);
```

and in the JSX:

```        
<h1>{displayUser?.name}<h1>
```
