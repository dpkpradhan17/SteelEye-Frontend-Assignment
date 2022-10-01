Based on the code below answer the following queries:

1. Explain what the simple List component does.
2. What problems / warnings are there with code?
3. Please fix, optimize, and/or modify the component as much as you think is necessary.

Answers:
1 -->
A list is used to store multiple items of the same and different datatype under a variable. It is basically an array. We can perform various operations on it such as .map() to loop over the list/array to list down all the elements. There is also a memo method used in the list component. React reuses the memoized content from the same reference. It makes the website speed and performance more efficient and better.

*****************************************************************


2 --->
In List component setSelectedIndex should be the second parameter otherwise our app throws an error. In DefaultProps of the same component items should not be null because it is falsy value it cannot be mapped.It should have an aaray instead, for example

WrappedListComponent.defaultProps = {
items: [items1, items2, items3],
};

The syntax of propTypes in List component is wrong it should be like this

WrappedListComponent.propTypes = {
items: PropTypes.arrayOf(
PropTypes.shape({
text: PropTypes.string.isRequired,
})),};


******************************************************************

3--->
import React, { useState, useEffect, memo } from "react";
import PropTypes from "prop-types";

// Single List Item
const WrappedSingleListItem = ({ index, isSelected, onClickHandler, text }) => {
return (
<li
style={{ backgroundColor: isSelected ? "green" : "red" }}
onClick={onClickHandler(index)}
key={index}>
{text}

);
};

WrappedSingleListItem.propTypes = {
index: PropTypes.number,
isSelected: PropTypes.bool,
onClickHandler: PropTypes.func.isRequired,
text: PropTypes.string.isRequired,
};

const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({ items }) => {
const [selectedIndex, setSelectedIndex] = useState(items);

// useEffect(() => {
// setSelectedIndex(null);
// }, [items]);

const handleClick = (index) => {
setSelectedIndex(index);
};

return (
<ul style={{ textAlign: "left" }}>
{items.map((item, index) => (
<SingleListItem
onClickHandler={() => handleClick(index)}
text={item.text}
index={index}
isSelected={selectedIndex}
/>
))}

);
};

WrappedListComponent.propTypes = {
items: PropTypes.arrayOf(
PropTypes.shape({
text: PropTypes.string.isRequired,
})
),
};

WrappedListComponent.defaultProps = {
items: [items1, items2, items3],
};

const List = memo(WrappedListComponent);

export default List;