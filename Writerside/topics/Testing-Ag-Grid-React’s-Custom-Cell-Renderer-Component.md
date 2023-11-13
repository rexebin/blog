# Testing Ag-Grid React's Custom Cell Renderer Component

I am on a journey to acquire the necessary skills for testing software. I will document the techniques and tricks I learned via blog posts.

[My first post](Understanding-Unit-Test-From-The-Best-Book-On-The-Topic.md) on testing explains what is a unit test and what makes a test good, the knowledge I gained from reading UNIT TESTING: PRINCIPLES, PRACTICES, AND PATTERNS by Vladimir Khorikov.

### Subject Under Test(sut):
a generic cell renderer component for Ag-Grid React renders any given component of type `FC<ICellRendererParams>`.

### Behaviour
The component being rendered is a small button that uses formatted cell value or cell value as its text. When the user clicks the button, it calls the specified action.

### Code

```Typescript
// GenericCellRenderer.spec.tsx
import { render } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { ColDef, ValueFormatterParams } from 'ag-grid-community';
import { AgGridReact } from 'ag-grid-react';
import { GenericCellRenderer } from './GenericCellRenderer';
import { useSmallButton } from './useSmallButton';

describe('Text button renderer', function () {
  type TestData = {
    totalReceived: string;
  };
  const rowData: TestData[] = [{ totalReceived: '100' }, { totalReceived: '200' }];

  function TestComponent<TestData>({
    action,
    valueFormatter,
  }: {
    action: (data: TestData) => void;
    valueFormatter?: (param: ValueFormatterParams) => string;
  }) {
    const SmallButton = useSmallButton<TestData>(action);
    const colDefs: ColDef[] = [
      {
        field: 'totalReceived',
        headerName: 'Total Received',
        valueFormatter: valueFormatter,
        cellRenderer: 'genericCellRenderer',
        cellRendererParams: {
          Component: SmallButton,
        },
      },
    ];

    return (
      <AgGridReact
        columnDefs={colDefs}
        rowData={rowData}
        frameworkComponents={{
          genericCellRenderer: GenericCellRenderer,
        }}
      />
    );
  }

  it('should render button with value as its text and trigger specified action on click', async function () {
    const action = jest.fn();
    const { findByText } = render(<TestComponent action={action} />);
    const button = await findByText('100');

    userEvent.click(button);
    expect(action).toHaveBeenCalledTimes(1);
    expect(action).toHaveBeenCalledWith({ totalReceived: '100' });
  });

  it('should render button with formatted value and trigger specified action on click', async function () {
    const action = jest.fn();
    const { findByText } = render(
      <TestComponent action={action} valueFormatter={(params) => `${params.value} USD`} />
    );
    const button = await findByText('100 USD');

    userEvent.click(button);
    expect(action).toHaveBeenCalledTimes(1);
    expect(action).toHaveBeenCalledWith({ totalReceived: '100' });
  });
});
```

```Typescript
// GenericCellRenderer.tsx
import { ICellRendererParams } from 'ag-grid-community';
import React, { FC } from 'react';

export function GenericCellRenderer<T>({
  Component,
  ...props
}: ICellRendererParams & {
  Component: FC<ICellRendererParams>;
}) {
  return <Component {...props} />;
}
```

```Typescript
// useSmallButton.tsx
import { Button } from '@mui/material';
import { ICellRendererParams } from 'ag-grid-community';

export const useSmallButton = <T,>(action: (data: T) => void) => {
  return ({ value, valueFormatted, data }: ICellRendererParams) => {
    return value !== null ? (
      <Button size={'small'} color={'primary'} variant={'text'} onClick={() => action(data)}>
        {valueFormatted ?? value}
      </Button>
    ) : null;
  };
};
```

### Notes

1. the test shows how to use the cell renderer in a `TestComponent`, which is sweet, one of the benefits of testing

1. the test must use the async query `findByText` instead of `getByText`. `findByText` will continuously try to search for the DOM until it finds the element or timeout. `getByText` doesn’t work (button not found error) here because the way Ag-Grid renders its cell renderers.

1. use `userEvent` to mimic the user’s click action, which emits a host of events, same as it happens when a user clicks the button in the browser.

1. the `action` is a spy to get asserted because this is the expected behaviour of the sut. I did not directly assert the other expected behaviour, rendering the button with value as its text. But it is tested because the button is used to trigger the event in the test.

1. no mocks. The test is rendering the `AgGridReact` component and the `Button` component from `@mui/material`

### Side Notes
The beauty about React is that it embraces functional programming. Its building block is function and encourages the use of pure function. So we can compose simple and pure functions together to make powerful yet simple, elegant and easy to test components.

In this instance, I used a generic cell renderer component to bridge between Ag-Grid’s API and my button component. The useSmallButton hook takes an function and returns a component that uses input. With this generic cell renderer component, I can render anything in a cell. I think Ag-Grid could implement such a generic component to make its API easier to use if not already.

My favourite feature of JavaScript is the spread/rest operator. With this operator, the props of a functional React component is like a contract, an interface, really elegant and powerful.