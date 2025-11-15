![FastAPI](https://cdn.svglogos.dev/logos/fastapi-icon.svg)

---

# FastAPI Project Structure
[Structuring a FastAPI Project: Best Practices](https://dev.to/mohammad222pr/structuring-a-fastapi-project-best-practices-53l6)

---

Path(..., ) means required
Path(10, ) means optional with default value

# Installation
```bash
pip install "fastapi[standard]"
```

---

# Run Live Server
```bash
fastapi dev main.py
```

---

# Init
```python
from fastapi import FastAPI

app = FastAPI()


@app.get('/')
async def root():
    return {'message': 'Hello World'}
```

---

# Data Types
- str
- int
- float
- bool (true, false, yes, no, 1, 0, on, off)
- list[], list[str]
- date (YYYY-MM-DD)
- datetime (2025-10-29T14:00:00)

---

# 3 Methods to Request Data
- Path parameter (Required)
- Query parameter
- Request body (JSON)

---

# 1. Path Parameters
- Required
- `https://.../items/123`
```python
@app.get('/items/{item_id}')
async def read_item(item_id: int):
    return {'item_id': item_id}
```

## Extra Validation
```python
from fastapi import Path
from typing import Annotated

@app.get('/items/{username}/{item_id}')
async def read_item(
	username: Annotated[
        str,
        Path(
	        title='Username',
            min_length=3,
            max_length=20,
            pattern='^[a-zA-Z0-9_-]+$'
        )
    ]
    
    item_id: Annotated[
        int, 
        Path(
            title='The ID of the item to get',
            description='Must be between 1 and 1000',
            ge=1,  # gt >
            le=1000,  # lt <
        )
    ]
):
    return {'username': username, 'item_id': item_id}
```

---

# 2. Query Parameters
- Appear after ?
- `https://.../items/123?q=hello&size=5`

## Required
```python
@app.get('/search/')
async def search(q: str):
    return {'q': q}
```

## Not Required (Default Value)
```python
@app.get('/items/')
async def read_items(skip: int = 0):
    return {'skip': skip}
```

## Not Required (Optional)
```python
@app.get('/items/{item_id}')
async def read_item(q: str | None = None):
    if q:
        return {'q': q}
    return {'q': 'sample'}
```

## Extra Validation
```python
from typing import Annotated
from fastapi import Query

@app.get('/items/')
async def read_items(
    username: Annotated[
	    str | None,  # Input can be str or None
	    Path(
	        title='Username',
	        description='3–20 chars',
	        
	        min_length=3,
	        max_length=20,
	        
	        pattern='^[a-zA-Z0-9_-]+$',
	        example='colin_leong',
	        deprecated=False,  # Show in doc
	        alias='user-name',
	        default='jason'
	    )
	] = None  # Default value
	
	limit: Annotated[
		int,
		Query(
			default=10,
			ge=1,
			le=100,
			example=20
		)
	]
	
	tags: Annotated[
		list[str],
		Query(
			default=[],
			min_length=1,
			max_length=10,
			unique_items=True
		)
	]
):
    return {'username': username, 'limit': limit, 'tags': tags}

```

---

# 3. Request Body (Pydantic Models)
- Request from JSON instead of URL

```python
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    price: float | None = None  # Can omit
```

## Constraints, Validations
```python
from pydantic import Field

class Item(BaseModel):
    name: str = Field(
        ...,  # No default value
        
        title='Item Name',
        description='The name of the item',
        
        min_length=3,
        max_length=50,
        
        pattern='^[A-Za-z0-9 _-]+$',
        alias='item_name',
        deprecated=False,
        example='FastAPI Book'
    )
    
    price: float = Field(
        10.9,  # Default value
        gt=0,
        lt=10000,
        multiple_of=0.01,
        example=19.99
    )
    
    tags: list[str] = Field(
	    min_length=1,
	    max_length=5,
	    unique_items=True
	)
	
	created_at: datetime = Field(
		default_factory=datetime.utcnow
	)
```

## Sample Usage
- Need to convert to dict to modify
- If not, can directly return item, FastAPI automatically convert to JSON
```python
@app.post('/items/')
async def create_item(item: Item):
    item_dict = item.dict()  # 
    
    if item.tax is not None:
        price_with_tax = item.price + item.tax
        item_dict.update({'price_with_tax': price_with_tax})
    return item_dict
```

---

# Alias Parameter
- Instead of searching for `?item_query=abc`, search for `?item-query=abc`
- Because `item-query` invalid Python naming
```python
@app.get('/items/')
async def read_items(
	item_query: Annotated[str | None, Query(alias='item-query')] = None
):
    return {'item query': item_query}
```

---

# Enums
- Automatic validation (Returns 422 error)
- Can also use `size.value == 'small`
```python
from enum import Enum

class CoffeeSize(str, Enum):
    small = 'small'
    medium = 'medium'
    large = 'large'


@app.get('/order/{size}')
async def order_coffee(size: CoffeeSize):
    if size is CoffeeSize.small:
        return {'size': size, 'message': 'A small coffee'}
    
    if size is CoffeeSize.large:
        return {'size': size, 'message': 'A large coffee'}
    return {'size': size, 'message': 'A medium coffee'}
```

---

# File Path
```python
@app.get('/files/{file_path:path}')
async def read_file(file_path: str):
    return {'file_path': file_path}
```

---

# Custom Validation
```python
from typing import Annotated
from pydantic import AfterValidator

# Validation function
def must_be_positive(value: int):
    if value <= 0:
        raise ValueError('Value must be positive')
    return value


@app.get('/square/')
async def square_number(
    x: Annotated[int, AfterValidator(must_be_positive)]
):
    return {'x': x, 'square': x ** 2}
```

---



