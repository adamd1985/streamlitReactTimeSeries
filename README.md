# Installation and Execution

Create conda env: `conda create --name ts python=3.9`

go to frontend and run `npm run build` this will create a *build* folder which will be accessed by the streamlit.

go to streamlit, install all requirements `conda install --file requirements.txt`

run the streamlit: `streamlit run app.py`

# Integration of Streamlit and React

In react we import the library: `streamlit-component-lib`

The component to be shared in streamlit is defined as follows (note the streamlit constructs):

```tsx
class App extends StreamlitComponentBase<State> {
  public state = { data: APPL_TS };

  public render = (): ReactNode => {
    return (
      <div>
        
      </div>
    );
  };
}
export default withStreamlitConnection(App);
```

In streamlit we import it as follows:

```python
import streamlit as st
import streamlit.components.v1 as components

_ts_component = components.declare_component(
    "ts_component", path="../frontend/build",
)
```

This allows us to built the UI seperately from the python code.

## Passing data across.

Data is read into the React component directly:

```tsx
const title = this.props.args?.title ?? 'temp';
const data = this.props.args?.data ?? APPL_TS;
```

This is provided when stream lit calls the component and passes properties:

```python
def load_component():
  return _app(data=APPL_TS, title="APPL")
```

The app passes data back into streamlit using:
```tsx
Streamlit.setComponentValue(e.target.value);
```

Each time this is done, the streamlit app is reloaded.

