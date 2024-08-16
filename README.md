# React + Vite

This template provides a minimal setup to get React working in Vite with HMR and some ESLint rules.

Currently, two official plugins are available:

- [@vitejs/plugin-react](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react/README.md) uses [Babel](https://babeljs.io/) for Fast Refresh
- [@vitejs/plugin-react-swc](https://github.com/vitejs/vite-plugin-react-swc) uses [SWC](https://swc.rs/) for Fast Refresh

Step 1 -> For Styled component
install styled component librery
#npm i styled-components
->Component Level
use of -> Create Style component import "styled" and put "." and then add your tag do the styles there. One thing to remember that as it'll return a component then we need to store in variable with Uppercase. Not only the tag name we can write any name there for use.
What ever you write the styles here it'll extract in the current component scope.
Example :

import styled from "styled-components";
function App() {
const H1 = styled.h1`  font-size: 30px;
    font-weight: 600;
    color: blue;`;
return (

<div>
<H1>Hello world</H1>
</div>
);
}
Note: Here it'll generate the class css with unique value.
If you want the vs-code extention for styled-component use "vscode-styled-components"

->Global Level
Just create a Global Styles by using "createGlobalStyle" with "`" direct put you styles here. store in a variable and export it as well. At the time using, just remember one thing it should self contained.
Example:
<GlobalStyles/>
->Use css in styled function
Example

import styled, { css } from "styled-components";
function App() {
const test = css`  text-align: center;`;

const StyledApp = styled.div`  background-color: var(--color-blue-100);
    padding: 20px;
    ${test}`;
Note: in JSX format we can write in styled component
Not only this things we can pass through the props as well just profide the proper props name with value

Step 2 -> Routing

# npm i react-router-dom@6

work with router dom

Step 3 -> add icons

# npm i react-icons

and add the react icons

Step 4 -> Add ReactQuery for remote state management

# npm i @tanstack/react-query@4

Use -
first create a Query client and provide to app environment
first set time by creating query client

Example :
import { QueryClient, QueryClientProvider } from "@tanstack/react-query";

function App() {
const queryClient = new QueryClient({
defaultOptions: {
mutations: 60 \* 1000,
},
});
return (
<QueryClientProvider client={queryClient}>
<BrowserRouter>
<Routes>
<Route element={<AppLayout />}>
<Route path="dashboard" element={<Dashboard />} />
</Route>
</Routes>
</BrowserRouter>
</QueryClientProvider>
);
}

After that we need to use the react query for remote fetching

1. only for get the data from remote server
   const reseved = useQuery({
   queryKey: ["users"],
   queryFn: getUsers,
   });
   console.log(reseved)

and here we can destracture the data like

const {isLoading,data,error} = useQuery 2. For deleting we use
Look the example bellow : after that we need to update and quick fetch query client, to call existing query client we use useQueryClient hook. and provide the key there.

const query = useQueryClient(); // for updated query cache
const { isLoading, mutate } = useMutation({
mutationFn: (id) => deleteUser(id),
onSuccess: query.invalidateQueries({
queryKey:["user"]
}),
});

and use mutate(id) in onClick event listner

Step -5 React Query Devtools use

# npm i @tanstack/react-query-devtools@4

After instal enable it
<QueryClientProvider client={queryClient}>
<ReactQueryDevtools initialIsOpen={false} />
<BrowserRouter>
<Routes>
<Route element={<AppLayout />}>
<Route path="dashboard" element={<Dashboard />} />
</Route>
</Routes>
</BrowserRouter>
</QueryClientProvider>

step- 6 working with data
it's deficult to calculate and handle the date function we can use

# npm i date-fns

step- 7 working with notification we want to show
we can use react-hot-toast

# npm i react-hot-toast

use - 1st we need to add the toaster in component tree

and provide the design how you want to show if success and error timing with styles we need to provide the details
</BrowserRouter>
<Toaster
position="top-center"
gutter={12}
containerStyle={{ margin: "8px" }}
toastOptions={{
          success: {
            duration: 3000,
          },
          error: {
            duration: 5000,
          },
          style: {
            fontSize: "16px",
            backgroundColor: "green",
            maxWidth: "500px",
            padding: "16px 20px",
          },
        }}
/>
</QueryClientProvider>

Now we can any where in our application just by using toast

toast.error(err.message);
toast.success("Deleted successfully");

step - 8 Handing the form with proper validation is dificult
we can use react-hook-form

# npm i react-hook-form@7

for working with form we useForm() hook
register -> track all the input field with their values
handleSubmit -> this function after geeting the data what we want to do.
reset -> for clearing the form data, we need to just call the reset() fn
const { register, handleSubmit, reset } = useForm();

const onSubmitCustomHandler = (data) => {
console.log(data); // we can get the all the form data and we can do what we like
};

<form onSubmit={handleSubmit(onSubmitCustomHandler)}>
            <input id="username" {...register("username")} />
            <input id="password" {...register("password")} />
          </form>

Validation ->
just we need to execute the fn that exist in register fn and provide the errors to handleSubmit fn. falow the example

function onErrorOcured(errors){
console.log(errors)
}

 <form onSubmit={handleSubmit(onSubmitCustomHandler,errors)}>
            <input
              id="username"
              {...register("username", {
                required: " this field is mandatory",
                min:{
                  value: 1,
                  message: "Minimum 1 value should be here"
                }
              })}
            />
            <input id="password" {...register("password"),{
              maxLength:"Length should be less then 10",
              validate: (pass) => pass >99999 || "Error msg write here"
            }} />
          </form>
Lot much more things we need to do here
const { register, handleSubmit,errors } = useForm();
         <input
              id="username"
              {...register("username", {
                required: " this field is mandatory",
                min:{
                  value: 1,
                  message: "Minimum 1 value should be here"
                }
              })}
            />
{errors?.usename.message && <Error>{errors.username.message}</Error>}
