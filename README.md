# Food Ordering Web App

This project is a beginner-friendly full-stack food ordering application built with:

- FastAPI
- Firebase Firestore
- HTML
- CSS
- JavaScript

## 1. System Architecture

### Backend

- FastAPI handles API routes.
- Pydantic validates request and response data.
- Firebase Firestore stores food items and orders.
- A small Firestore service file keeps database code reusable.

### Frontend

- `index.html` creates the page structure.
- `styles.css` handles layout, responsiveness, and animations.
- `app.js` fetches food data, manages cart state, places orders, and shows order history.

## 2. Folder Structure

```text
food/
├── backend/
│   ├── app/
│   │   ├── db/
│   │   │   └── firebase.py
│   │   ├── models/
│   │   │   ├── food.py
│   │   │   └── order.py
│   │   ├── routers/
│   │   │   ├── foods.py
│   │   │   └── orders.py
│   │   ├── services/
│   │   │   └── firestore_service.py
│   │   ├── config.py
│   │   └── main.py
│   ├── .env.example
│   └── requirements.txt
├── frontend/
│   ├── app.js
│   ├── index.html
│   └── styles.css
└── README.md
```

## 3. Setup Instructions

### Backend setup

1. Open a terminal inside `backend`.
2. Create a virtual environment:

```bash
python -m venv venv
```

3. Activate the environment:

```bash
venv\Scripts\activate
```

4. Install dependencies:

```bash
pip install -r requirements.txt
```

5. Copy `.env.example` to `.env`.
6. Add your Firebase project ID.
7. Place your Firebase service account file inside the `backend` folder.

### Run the backend

```bash
uvicorn app.main:app --reload
```

The API will run at:

```text
http://127.0.0.1:8000
```

Swagger docs:

```text
http://127.0.0.1:8000/docs
```

### Frontend setup

Open a second terminal inside `frontend` and run:

```bash
python -m http.server 5500
```

Then open:

```text
http://127.0.0.1:5500
```

## 4. Sample API Testing

### Create a food item with image upload

```bash
curl -X POST "http://127.0.0.1:8000/api/foods/" ^
  -F "name=Cheese Burger" ^
  -F "description=Juicy grilled burger with cheese" ^
  -F "price=8.99" ^
  -F "category=Burger" ^
  -F "available=true" ^
  -F "image=@burger.jpg"
```

### Get all food items

```bash
curl "http://127.0.0.1:8000/api/foods/"
```

### Place an order

```bash
curl -X POST "http://127.0.0.1:8000/api/orders/" ^
  -H "Content-Type: application/json" ^
  -d "{\"customer_name\":\"Alex\",\"customer_email\":\"alex@example.com\",\"items\":[{\"food_id\":\"food123\",\"name\":\"Cheese Burger\",\"price\":8.99,\"quantity\":2}],\"total_amount\":17.98,\"status\":\"Placed\"}"
```

### Get order history

```bash
curl "http://127.0.0.1:8000/api/orders/"
```

## 5. Debugging Guide

### ModuleNotFoundError

Problem:
Python cannot find installed packages.

Fix:

- Make sure the virtual environment is activated.
- Run `pip install -r requirements.txt` again.
- Start the server from the `backend` folder.

### Firebase errors

Problem:
Firebase credentials or project settings are incorrect.

Fix:

- Check if `crud.json` exists in `backend`.
- Verify `FIREBASE_CREDENTIALS_PATH` is correct.
- Verify `FIREBASE_PROJECT_ID` matches your Firebase project.
- Make sure Firestore is enabled in Firebase Console.

### Internal Server Error

Problem:
The backend raised an unexpected exception.

Fix:

- Read the terminal error message carefully.
- Check if request data matches the Pydantic model.
- Verify Firebase is connected successfully.
- Check API in `/docs` to confirm the request body format.

### CORS issues

Problem:
The browser blocks frontend requests to the backend.

Fix:

- Make sure `FRONTEND_ORIGIN` matches the frontend URL exactly.
- Example: `http://127.0.0.1:5500`
- Restart the FastAPI server after editing `.env`.

## 6. Notes for Beginners

- Start the backend first.
- Add food items using the upload form in the frontend, Swagger, or `curl`.
- Then open the frontend and test cart and order flow.
- If the page shows no food items, your `foods` collection is still empty.
- Uploaded food images are served from `http://127.0.0.1:8000/uploads/...`.
