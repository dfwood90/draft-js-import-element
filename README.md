# DraftJS: Import Element to ContentState

This is a module for [DraftJS](https://github.com/facebook/draft-js) that will convert an HTML DOM Element to editor content.

It was extracted from [React-RTE](https://react-rte.org) and placed into a separate module for more general use. Hopefully it can be helpful in your projects.

## Installation

    npm install --save draft-js-import-element

This project is still under development. If you want to help out, please open an issue to discuss or join us on [Slack](https://draftjs.slack.com/).

## Usage

`stateFromElement` takes a DOM node `element` and returns a DraftJS [ContentState](https://facebook.github.io/draft-js/docs/api-reference-content-state.html).

```javascript
import {stateFromElement} from 'draft-js-import-element';
const contentState = stateFromElement(element);
```

### Options

You can optionally pass a second `Object` argument to `stateFromElement` with the following supported properties:

- `blockRenderFilter`: Callback function to set block type based on HTML tag name. Example:

        stateFromElement(element, {
            // Should always return null or a valid block type. `tagName` is a lowercase version of the HTML tag name
            blockRenderFilter: (tagName) => {
                switch (tagName) {
                    // This example forces all elements with the h6 tag to be assigned the block quote block type
                    case 'h6':
                        // You can import `BLOCK_TYPE` from Draft.js' `draft-js-utils` or use a string
                        return BLOCK_TYPE.BLOCKQUOTE;
                    default:
                        return null;
                }
            }
        });

- `elementStyles`: HTML element name as key, DraftJS style string as value. Example:

        stateFromElement(element, {
          elementStyles: {
            // Support `<sup>` (superscript) tag as style:
            'sup': 'SUPERSCRIPT'
          }
        });

- `filterBlockData`: Callback function to set custom block data based on block element. Example:
        
        stateFromElement(element, {
          // `data` is a JS object, `element` is the HTML node for the block 
          filterBlockData: (data, element) {
            // Check if the element has the inline style `text-align`
            if (element.style.textAlign) {
              // Set `textAlign` to equal the `text-align` CSS property in the block data map
              data.textAlign = element.style.textAlign;
            }
          
            // Should always return `data`
            return data;
          }
        });

## License

This software is [BSD Licensed](/LICENSE).
