# tests-create_operation.py

import os
import requests

BASE_URL = os.getenv("BASE_URL", "https://zimmeroms-staging.azurewebsites.net/operations")
TOKEN = os.getenv("API_TOKEN")

def test_create_operation():
    if not TOKEN:
        raise RuntimeError("API_TOKEN is missing")

headers = {
        "Authorization": f"Bearer {TOKEN}",
        "Content-Type": "application/json",
    }

# Replace with the real required payload for your API
    payload = {
        "name": "Test Operation"
    }

create_response = requests.post(BASE_URL, json=payload, headers=headers)
    assert create_response.status_code in (200, 201), create_response.text

created = create_response.json()
    operation_id = created.get("id")
    assert operation_id, "Create API did not return an operation id"

view_response = requests.get(f"{BASE_URL}/{operation_id}", headers=headers)
    assert view_response.status_code // 100 == 2, view_response.text

viewed = view_response.json()
    assert str(viewed.get("id")) == str(operation_id)

return {"operation_id": operation_id}
