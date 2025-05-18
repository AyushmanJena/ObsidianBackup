# Flexbox
display: flex;
flex-direction: column; // row, row-reverse, column-reverse
flex-wrap: wrap;

flex: 2;               // can be given to flex items : specifies how much unit space each item takes
flex: 2 100px;       // minimum size value

flex can have 3 values : 
1. flex-grow => flex: 2; above
2. flex-shrink  => when the flex items are overflowing their container.
3. flex-basis => 100px above 

### align-items
align-items  -> specifies where the flex items sit on the cross axis.
values :
1. stretch : fills the parent element 
2. center 
3. flex-start
4. self-start
5. start
6. flex-end
7. self-end
8. end

### justify-content
justify-content -> specifies where the flex items sit on the main axis.
values : 
1. start
2. end/flex-end
3. left
4. right
5. center 
6. space-around
7. space-between

for flex item :
order: 1;  // changing the layout order of flex items
- Flex items with higher specified order values will appear later in the display order than items with lower order values.

### Nested Flex Boxes
