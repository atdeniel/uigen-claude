# Test Report: use-auth.ts

## Summary

| Metric | Value |
|--------|-------|
| Test File | `src/hooks/__tests__/use-auth.test.ts` |
| Target | `src/hooks/use-auth.ts` |
| Total Tests | 18 |
| Passed | 18 |
| Failed | 0 |
| Duration | ~363ms |

## Test Coverage

### Initial State (2 tests)

| Test | Status |
|------|--------|
| Returns `isLoading` as false initially | ✅ |
| Returns `signIn` and `signUp` functions | ✅ |

### signIn (8 tests)

| Test | Status |
|------|--------|
| Sets `isLoading` to true during sign in | ✅ |
| Calls `signInAction` with email and password | ✅ |
| Returns result from `signInAction` | ✅ |
| Redirects to anonymous work project on success when anon work exists | ✅ |
| Redirects to most recent project when no anon work | ✅ |
| Creates new project when no anon work and no existing projects | ✅ |
| Does not redirect on failed sign in | ✅ |
| Handles empty messages in anon work | ✅ |

### signUp (6 tests)

| Test | Status |
|------|--------|
| Sets `isLoading` to true during sign up | ✅ |
| Calls `signUpAction` with email and password | ✅ |
| Returns result from `signUpAction` | ✅ |
| Redirects to anonymous work project on success when anon work exists | ✅ |
| Creates new project for new user with no anon work | ✅ |
| Does not redirect on failed sign up | ✅ |

### Error Handling (2 tests)

| Test | Status |
|------|--------|
| Resets `isLoading` to false even on signIn error | ✅ |
| Resets `isLoading` to false on signUp error | ✅ |

## Mocked Dependencies

- `next/navigation` - `useRouter` (push method)
- `@/actions` - `signIn`, `signUp`
- `@/lib/anon-work-tracker` - `getAnonWorkData`, `clearAnonWork`
- `@/actions/get-projects` - `getProjects`
- `@/actions/create-project` - `createProject`

## Test Scenarios Covered

### Authentication Flow
1. **Success path with anonymous work**: User signs in/up with unsaved anonymous work, which gets saved to a new project
2. **Success path with existing projects**: User signs in and gets redirected to their most recent project
3. **Success path for new user**: User signs up with no prior work, a new project is created
4. **Failure path**: Authentication fails, no redirect occurs

### State Management
- Loading state transitions correctly during async operations
- Loading state resets on both success and failure
- Loading state resets even when exceptions are thrown

## Running Tests

```bash
npm run test -- --run src/hooks/__tests__/use-auth.test.ts
```
