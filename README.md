# MyForm Component

The `MyForm` component is a React functional component that handles form submission with loading, success, and error states.

## Imports

```tsx
import { useState, useActionState } from 'react';
```

- `useState`: A React hook for managing state.
- `useActionState`: A custom hook for managing complex state transitions.

## State Variables

- `formData`: An object containing the form data.
- `submittedData`: Stores the data returned from the server after form submission.
- `submitState`: An object containing the state of the form submission (loading, success, error).

## Initial State

```tsx
const initialState = { loading: false, success: false, error: false };
```

## Action Function

```tsx
const action = (state: typeof initialState, payload: Partial<typeof initialState>) => ({ ...state, ...payload });
```

## Form Submission Handler

```tsx
const handleSubmit = async (e: { preventDefault: () => void; }) => {
  e.preventDefault();
  setSubmitState({ loading: true });

  try {
    const response = await fetch('http://localhost:3006/api/general/ss', {
      method: 'POST',
      body: JSON.stringify(formData),
      headers: {
        'Content-Type': 'application/json',
      },
    });

    const result = await response.json();
    setSubmittedData(result);
    setSubmitState({ loading: false, success: true });
  } catch (error) {
    setSubmitState({ loading: false, error: true });
  }
};
```

## JSX Structure

```tsx
return (
  <form onSubmit={handleSubmit}>
    <input
      type="text"
      value={formData.name}
      onChange={(e) => setFormData({ ...formData, name: e.target.value })}
    />
    <button type="submit" disabled={submitState.loading}>Submit</button>
    {submitState.loading && <p>Loading...</p>}
    {submitState.success && <p>Form submitted successfully!</p>}
    {submitState.error && <p>There was an error submitting the form.</p>}
  </form>
);
```

## Export

```tsx
export default MyForm;
```

The component is exported as the default export.
