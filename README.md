
# Anonymous Arrow Functions in React useState

useState is used to decide the initial value of a variable, usually, something like an array/object, an empty string or a boolean works, however, there are rare cases where you'd want to use an anonymous arrow function in order to set the initial value.


## Example 1: displayUser


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
etc...

## Example 2: saving the switch toggle state to localStorage
- in this example, I save the state of wether the switch is toggled false or true to local storage, the reason why I do that is because in this app "Care", darkMode is saved to local storage in a similiar fashion as well, so it would be odd to not also save the toggle state of the switch to local storage, right? If dark mode is saved to local storage, the toggled switch has to be saved as well.

in this example, i'm using an anonymous function and I'm sort of doing a mini-algorithm here.
notice, the state is equal to localStorage.getItem('switchState')
if state is equal to null, meaning if there are no cookies saved/nothing from darkmode/switch state saved to local storage, return false (aka, return untoggled switch)
if it's not equal to null, we continue to this logic:
if the state is true, meaning if the switch is toggled on (dark mode is on) return true, else return false. 
```
 const [switchState, setSwitchState] = useState(() => {
    let state = localStorage.getItem("switchState");
    if (state !== null) {
      return state === "true" ? true : false;
    }
    return false;
  });
```

the rest of the logic is handeled here:

```
  const handleThemeChange = () => {
    setSwitchState(switchState === true ? false : true);

    if (darkMode === "light") {
      setDarkMode("dark");
      localStorage.setItem("darkMode", "dark");
      localStorage.setItem("switchState", true);
    } else {
      setDarkMode("light");
      localStorage.setItem("darkMode", "light");
      localStorage.setItem("switchState", false);
    }
  };
```

and the JSX: 
```
<Switch
  checked={switchState}
   onChange={handleThemeChange}
        />
```
note that this logic by itself isn't enough to handle darkMode, but it is enough to save a switch to local storage, or to give an idea as to how to save dark mode into local storage, darkMode is a whole tutorial by itself, since it requires use of useContext.
