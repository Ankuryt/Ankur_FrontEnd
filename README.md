# Ankur_FrontEnd
The given code defines a simple List component that renders a list of items with a single selected item at a time. The list is created using an unordered list (<ul>) and each item is rendered as a separate list item (<li>).

The List component takes an array of items as a prop, where each item is an object with a text property. The component uses the useState hook to keep track of the index of the selected item, initializing it to null. Whenever the items prop changes, the selected index is reset to null using the useEffect hook.

The List component then maps over the items array and renders a SingleListItem component for each item. The SingleListItem component takes the text and index properties as props and also receives isSelected and onClickHandler props from the List component.

The isSelected prop determines whether the item is selected or not, and the onClickHandler prop is a callback function that updates the selected index in the state with the index of the clicked item.

The SingleListItem component renders the item's text as the content of the list item (<li>) and changes the background color based on whether the item is selected or not. The onClick event is attached to the list item to trigger the onClickHandler callback when the item is clicked.

Overall, this List component can be used as a reusable component in React applications to render a list of items with a single selected item at a time.








There are a few problems and warnings with the given code:

In the SingleListItem component, the onClick event handler should be a function that returns the onClickHandler callback with the index as an argument, like this: onClick={() => onClickHandler(index)}. Otherwise, the onClickHandler will be immediately invoked when the component is rendered, which is not the intended behavior.

In the SingleListItem component, the isSelected prop is passed as a boolean value, but it should be passed as the index of the selected item. Otherwise, the background color will always be green, even when no item is selected. To fix this, the isSelected prop should be passed as isSelected === index.

In the WrappedListComponent propTypes, the items prop should be defined as PropTypes.arrayOf(PropTypes.shape({...})) instead of PropTypes.array(PropTypes.shape({...})).

The selectedIndex state in the WrappedListComponent is initialized without a default value, which can cause problems if the items prop is not passed as an array. To fix this, the useState hook should be initialized with a default value of null like this: const [selectedIndex, setSelectedIndex] = useState(null);.

In the WrappedListComponent component, the setSelectedIndex variable should be const [selectedIndex, setSelectedIndex] = useState(null); to correctly use the useState hook.

In the SingleListItem component, the onClickHandler prop is marked as isRequired, but it is not always passed as a prop in the List component, which can cause a warning. To fix this, the onClickHandler prop should have a default value of an empty function, like this: onClickHandler: PropTypes.func.

In the WrappedSingleListItem component, the onClickHandler prop is not marked as isRequired, but it is required for the component to work correctly. To fix this, the onClickHandler prop should be marked as isRequired, like this: onClickHandler: PropTypes.func.isRequired.

The List and WrappedListComponent components are unnecessarily wrapped with the memo higher-order component, as there are no expensive computations or prop comparisons that would benefit from memoization. However, this is not a warning or an error, but just a performance improvement that could be made.





import React, { useState, useEffect, useCallback } from 'react';
import PropTypes from 'prop-types';

// Single List Item
const SingleListItem = ({ index, isSelected, onClickHandler, text }) => {
  const handleClick = useCallback(() => onClickHandler(index), [index, onClickHandler]);

  return (
    <li
      style={{ backgroundColor: isSelected === index ? 'green' : 'red' }}
      onClick={handleClick}
    >
      {text}
    </li>
  );
};

SingleListItem.propTypes = {
  index: PropTypes.number.isRequired,
  isSelected: PropTypes.number,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

// List Component
const List = ({ items }) => {
  const [selectedIndex, setSelectedIndex] = useState(null);

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = useCallback(index => {
    setSelectedIndex(index);
  }, []);

  return (
    <ul style={{ textAlign: 'left' }}>
      {items.map((item, index) => (
        <SingleListItem
          key={index}
          onClickHandler={handleClick}
          text={item.text}
          index={index}
          isSelected={selectedIndex}
        />
      ))}
    </ul>
  );
};

List.propTypes = {
  items: PropTypes.arrayOf(
    PropTypes.shape({
      text: PropTypes.string.isRequired,
    })
  ).isRequired,
};

export default List;
