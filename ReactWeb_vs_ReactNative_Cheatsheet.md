# React Web vs React Native Cheatsheet

## Basic Components

### Web (React DOM)

```jsx
import React from 'react';

const WebComponent = () => {
    return (
        <div className="container">
            <h1>Welcome</h1>
            <p>This is a web component</p>
        </div>
    );
};
```

### Native (React Native)

```jsx
import React from 'react';
import { View, Text } from 'react-native';

const NativeComponent = () => {
    return (
        <View style={styles.container}>
            <Text style={styles.title}>Welcome</Text>
            <Text>This is a native component</Text>
        </View>
    );
};
```

## Styling

### Web (CSS)

```jsx
const WebStyleComponent = () => {
    return (
        <div className="card">
            <img src="/profile.jpg" className="profile-image" />
            <div className="content">
                <h2>John Doe</h2>
                <p>Web Developer</p>
            </div>
        </div>
    );
};
```

```css
/* CSS file */
.card {
    padding: 16px;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}
.profile-image {
    width: 100px;
    height: 100px;
    border-radius: 50%;
}
```

### Native (StyleSheet)

```jsx
import { StyleSheet } from 'react-native';

const NativeStyleComponent = () => {
    return (
        <View style={styles.card}>
            <Image 
                source={require('./profile.jpg')}
                style={styles.profileImage}
            />
            <View style={styles.content}>
                <Text style={styles.name}>John Doe</Text>
                <Text style={styles.role}>Mobile Developer</Text>
            </View>
        </View>
    );
};

const styles = StyleSheet.create({
    card: {
        padding: 16,
        borderRadius: 8,
        shadowColor: '#000',
        shadowOffset: { width: 0, height: 2 },
        shadowOpacity: 0.1,
        shadowRadius: 4,
    },
    profileImage: {
        width: 100,
        height: 100,
        borderRadius: 50,
    },
});
```

## Buttons

### Web

```jsx
const WebButton = () => {
    const handleClick = (event) => {
        console.log('Button clicked:', event);
    };

    return (
        <button
            onClick={handleClick}
            onMouseOver={() => console.log("Mouse over")}
        >
            Click me
        </button>
    );
};
```

### Native

```jsx
const NativeButton = () => {
    const handlePress = (event) => {
        console.log('Button pressed:', event);
    };

    return (
        <TouchableOpacity
            onPress={handlePress}
            onLongPress={() => console.log("Long press")}
        >
            <Text>Press me</Text>
        </TouchableOpacity>
    );
};
```

## Lists

### Web

```jsx
const WebList = ({ items }) => {
    return (
        <div className="list-container">
            {items.map((item) => (
                <div key={item.id} className="list-item">
                    <h3>{item.title}</h3>
                    <p>{item.description}</p>
                </div>
            ))}
        </div>
    );
};
```

### Native

```jsx
const NativeList = ({ items }) => {
    const renderItem = ({ item }) => (
        <View style={styles.listItem}>
            <Text style={styles.itemTitle}>{item.title}</Text>
            <Text>{item.description}</Text>
        </View>
    );

    return (
        <FlatList
            data={items}
            renderItem={renderItem}
            keyExtractor={(item) => item.id}
            onEndReached={() => console.log("End reached")}
            onEndReachedThreshold={0.5}
        />
    );
};
```

## Forms

### Web

```jsx
const WebForm = () => {
    const [formData, setFormData] = useState({
        username: "",
        email: "",
    });

    return (
        <form onSubmit={(e) => e.preventDefault()}>
            <input
                type="text"
                value={formData.username}
                onChange={(e) =>
                    setFormData({
                        ...formData,
                        username: e.target.value,
                    })
                }
                placeholder="Username"
            />
            <button type="submit">Submit</button>
        </form>
    );
};
```

### Native

```jsx
const NativeForm = () => {
    const [formData, setFormData] = useState({
        username: "",
        email: "",
    });

    return (
        <View style={styles.form}>
            <TextInput
                style={styles.input}
                value={formData.username}
                onChangeText={(text) =>
                    setFormData({
                        ...formData,
                        username: text,
                    })
                }
                placeholder="Username"
            />
            <TouchableOpacity onPress={() => console.log("Submit")}>
                <Text>Submit</Text>
            </TouchableOpacity>
        </View>
    );
};
```

## Key Differences Summary

| Feature         | React Web            | React Native                 |
|----------------|----------------------|------------------------------|
| Basic container| `<div>`              | `<View>`                     |
| Text           | `<h1>`, `<p>`        | `<Text>`                     |
| Styling        | CSS classes          | `StyleSheet.create()`        |
| Images         | `<img src="..." />`  | `<Image source={require()} />` |
| Buttons        | `<button>`           | `<TouchableOpacity>`         |
| Lists          | `Array.map()`        | `<FlatList>` or `<ScrollView>` |
| Input          | `<input>`            | `<TextInput>`                |
| Forms          | `<form>`             | `<View>` with inputs         |
| Events         | `onClick`, `onChange`| `onPress`, `onChangeText`    |