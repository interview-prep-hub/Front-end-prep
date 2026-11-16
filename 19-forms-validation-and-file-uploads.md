# 19. Forms, validation, and file uploads

Previous: [18. Query](18-tanstack-query-and-server-state.md) | Next: [20. API contracts](20-api-contracts-and-real-time-ui.md)

## A form is a workflow

Separate entered values, touched/dirty state, client validation, server validation, submission status, and canonical saved data. A field may be syntactically valid but fail a server business rule. Keep user input after failures and associate server field errors with the correct control.

Controlled inputs use React state as their source of truth; uncontrolled inputs use the DOM and can be read through forms/refs. Do not switch an input between defined and undefined values accidentally. An uncontrolled file input is normal because browsers restrict programmatic file selection.

Use native labels, input types, autocomplete, names, fieldsets, and useful constraints. Client validation improves feedback; server validation is mandatory. Decide when to validate, such as blur or submit, so users are not overwhelmed while typing.

## Schema example

```ts
import { z } from 'zod';

export const applicationSchema = z.object({
  houseId: z.string().min(1),
  email: z.string().email(),
  householdSize: z.number().int().min(1).max(20),
  message: z.string().trim().min(20).max(2000),
});
export type ApplicationInput = z.infer<typeof applicationSchema>;

export function validateApplication(form: FormData) {
  const rawSize = form.get('householdSize');
  return applicationSchema.safeParse({
    houseId: form.get('houseId'),
    email: form.get('email'),
    householdSize: typeof rawSize === 'string' && rawSize.trim() !== ''
      ? Number(rawSize) : undefined,
    message: form.get('message'),
  });
}
```

The sample avoids turning an empty numeric field into zero accidentally. It still needs business rules and server-authorized house access. A shared schema may express common shape rules; client code must not import private server services. See [Zod basics](https://zod.dev/basics) for parsing and error handling.

React Hook Form can manage registration, values, dirty state, errors, and submission with standard inputs; use its [getting-started guide](https://react-hook-form.com/get-started) for the installed version. Integrate a schema resolver deliberately and distinguish native input registration from controlled third-party widget adapters. Know the underlying React/native behavior before relying on a form library.

## Submission and concurrency

Prevent accidental duplicate submission while pending, but enforce idempotency on the server for operations that need it. A disabled submit button is not an authorization or deduplication mechanism. Announce progress and errors. On success, choose whether to reset, navigate, or retain values based on the user's next task.

For dependent fields, clear or revalidate incompatible choices. For dynamic lists, use stable item IDs so errors and input state do not move to another row. For multi-step forms, define when state persists, when it expires, and how logout/tenant switching clears it.

Autosave needs debounce, race handling, a saved/pending/error indicator, and a version/conflict policy. A late response must not mark newer unsaved input as saved. Save against a draft version or operation ID and reconcile deliberately.

## Files and rich input

Use FormData for multipart uploads; let the browser set its multipart boundary instead of manually setting an incomplete Content-Type. Check size/type for feedback, then enforce the actual constraints server-side. File extensions and client-reported MIME types are not proof of content safety.

Object URLs created for previews need revocation when no longer used. Large uploads may use signed storage URLs, progress, cancellation, and a final server confirmation. Keep storage credentials private. Uploaded rich text and filenames are untrusted display data; sanitize interpreted markup and avoid unsafe insertion.

## Practice and questions

Build a housing application with dynamic residents, attachment preview, cross-field validation, a server field-error response, and a simulated failed submission. Add one autosave race and fix it. Test keyboard flow and ensure errors remain discoverable without a toast.

**Client or server validation?** Both, for different reasons. **Why not store every keystroke in global state?** Draft ownership and update frequency usually favor the form boundary. **How do you confirm payment?** From an authoritative server/provider state, not a successful browser redirect or client callback alone.

Exit gate: submit, fail, recover, and resubmit without losing user work or confusing stale responses with current values.
