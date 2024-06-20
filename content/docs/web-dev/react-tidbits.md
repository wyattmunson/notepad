---
title: "React Tidbits"
description: ""
lead: ""
date: 2023-01-06T19:15:34-08:00
lastmod: 2023-01-06T19:15:34-08:00
draft: true
images: []
menu:
  docs:
    parent: ""
    identifier: "react-tidbits-8f3c1bf11ad0c25ff74cefce25fde030"
weight: 999
toc: true
---

For your coding pleasure

## React Router

### Navigate back to the previous page

```js
import { useNavigate } from "react-router-dom";

const SomeComponent = () => {
  const navigate = useNavigate();

  return <Button text="Back" clickAction={() => navigate(-1)} />;
};
```

### Redirect

```js
import { useHistory } from 'react-router-dom';

const Home = () => {
    const history = useHistory();
    return (
        <button onClick={() => history.push('/products')}>
    );
};
```

Alternately

```js
import { useNavigate } from "react-router-dom";

const Home = () => {
  const navigate = useNavigate();
  return (

    <button onClick={() => navigate('/products')}>

  );
};
```

### Navigate replaced Redirect in RR v6

```js
import { Navigate } from "react-router-dom";

<Route path="/redirect" element={<Navigate to="/error-page" />} />;
```

### Render route and pass props

```js
<Routes>
  <Route exact path="/" element={<TripList trips={trips} />} />
  <Route path="/add" element={<AddTrip />} />
</Routes>
```

## Random

### Old Alert

```js
<FormButton
  text="Danger Alert"
  className="btn-secondary"
  clickAction={() => setAlert("Record failed to save", "danger")}
/>
```
