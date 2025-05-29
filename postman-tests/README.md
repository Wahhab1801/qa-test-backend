# Postman Tests 

This folder contains automated Postman test collections for authentication and shift management APIs.

## Structure

```
postman-tests/
├── auth/
│   ├── login.json         # Tests for user login
│   └── register.json      # Tests for user registration
├── shift/
│   ├── add_shift.json     # Tests for adding new shifts (with/without token)
│   ├── get_shift.json     # Tests for retrieving shifts
│   └── remove_shift.json  # Tests for removing shifts by ID (valid, invalid ID)

```

## Running Tests with Newman

You can run any collection using [Newman](https://www.npmjs.com/package/newman):

### Authentication Tests

```bash
newman run postman-tests/auth/login.json
newman run postman-tests/shift/register.json
```

### Shift Tests
```bash 
### For security reason AuthToken isn't hardcoded in the code and not provided in the Readme
newman run postman-tests/shift/add_shift.json --env-var authToken=<Use the swagger /user/login to get the auth token>

### For /shift/getShift the server error reponse code is documented 501, but it is returning 500 instead
newman run postman-tests/shift/get_shift.json --env-var authToken=<Use the swagger /user/login to get the auth token>

### For /shift/removeShift the server error reponse code is documented 501, but it is returning 500 instead
#### /shift/removeShift doesn't delete shifts and providing invalidID returns 200 success instead of 501/500 server error. That's why this test will fail.
newman run postman-tests/shift/remove_shift.json --env-var authToken=<Use the swagger /user/login to get the auth token> --env-var validShiftId=<Existing shift ID>
```

---

## TODO

## Auth
### Register
- [ ] **Duplicate Email**: Expect 409 or 400
- [ ] **Invalid Email Format**: Expect 400
- [ ] **Weak/Short Password**: Expect 400
- [ ] **Missing Fields**: Expect 400
- [ ] **SQL Injection Attempt**: Expect 400 or sanitize input

### Login
- [ ] **Non-existent User**: Expect 401 or 400
- [ ] **Missing Fields**: Expect 400
- [ ] **Wrong Email Format**: Expect 400
- [ ] **Case-Insensitive Email**: Login should succeed

## Shift
### Add Shift
- [ ] **Missing Title**: Expect 400
- [ ] **Invalid Data Type**: Expect 400
- [ ] **Duplicate Shift**: Should return 409 if not allowed
- [ ] **Special Characters in Title**: Expect 200 or sanitization

### Get Shift
- [ ] **Valid Token**: Expect 200 and return array of shifts
- [ ] **Without Token**: Expect 401
- [ ] **Invalid Token**: Expect 403
- [ ] **No Shifts**: Return empty array

### Remove Shift
- [ ] **Invalid ID Format**: Expect 400 or 404
- [ ] **Nonexistent ID**: Expect 404 or 500
- [ ] **Already Deleted ID**: Should not crash, return 404
- [ ] **Missing ID**: Expect 400
- [ ] **Unauthorized Deletion**: Expect 403

---

## Enhancements

- Integrate with CI/CD pipelines (Jenkins) for automated regression testing.