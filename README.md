# FastAPI Backend with Automated Testing and CI/CD Integration 🚀

This project demonstrates how to set up a basic **FastAPI** backend, automate API testing with Python, and integrate tests into a **GitHub Actions CI/CD pipeline**. The primary goal is to understand backend API test automation, GitHub Actions for continuous testing, and broader DevOps workflows.

---

## Project Overview 📋

### Features
- **FastAPI Server**: Provides endpoints for basic mathematical operations - addition, subtraction, and multiplication. ➕ ➖ ✖️
- **Automated Testing**: Uses `requests` and `pytest` to test the API endpoints. ✅
- **CI/CD Pipeline**: Integrated with GitHub Actions for continuous testing on code changes. 🔄
- **Real-World Expansion**: Framework can be extended to include databases, authentication, and performance testing. 🏗️

---

## Setup Instructions ⚙️

### Task 1: Set Up the FastAPI Server 🖥️
1. **Install Required Packages**:
    ```bash
    pip install fastapi uvicorn
    ```
2. **Create the FastAPI Server**: Save the following code as `apiserver.py`:
    ```python
    from fastapi import FastAPI

    app = FastAPI()

    @app.get("/")
    def read_root():
        return {"Hello": "World"}

    @app.get("/add/{num1}/{num2}")
    def add(num1: int, num2: int):
        return {"result": num1 + num2}

    @app.get("/subtract/{num1}/{num2}")
    def subtract(num1: int, num2: int):
        return {"result": num1 - num2}

    @app.get("/multiply/{num1}/{num2}")
    def multiply(num1: int, num2: int):
        return {"result": num1 * num2}

    if __name__ == "__main__":
        import uvicorn
        uvicorn.run("apiserver:app", host="0.0.0.0", port=8000, reload=True)
    ```
3. **Run the Server**:
    ```bash
    python apiserver.py
    ```

---

### Task 2: Writing Automated Tests for the API 📝
1. **Install Requests**:
    ```bash
    pip install requests
    ```
2. **Create the Test Script**: Save the following code as `testAutomation.py`:
    ```python
    import requests

    testcases = [
        {"url": "http://localhost:8000/add/2/2", "expected": 4, "description": "Test addition of 2 and 2"},
        {"url": "http://localhost:8000/subtract/2/2", "expected": 0, "description": "Test subtraction of 2 from 2"},
        {"url": "http://localhost:8000/multiply/2/2", "expected": 4, "description": "Test multiplication of 2 and 2"}
    ]

    def test():
        for case in testcases:
            response = requests.get(case["url"])
            result = response.json()["result"]
            assert result == case["expected"], f"Test failed: {case['description']}. Expected {case['expected']}, got {result}"
            print(f"Test passed: {case['description']}")

        print("All tests passed!")

    test()
    ```
3. **Run the Tests**:
    ```bash
    python testAutomation.py
    ```

---

### Task 3: Enhancing the Tests with Pytest 🧪
1. **Install Pytest**:
    ```bash
    pip install pytest
    ```
2. **Create the Pytest Script**: Save the following code as `testAutomationPytest.py`:
    ```python
    import pytest
    import requests

    testcases = [
        ("http://localhost:8000/add/2/2", 4, "Test addition of 2 and 2"),
        ("http://localhost:8000/subtract/2/2", 0, "Test subtraction of 2 from 2"),
        ("http://localhost:8000/multiply/2/2", 4, "Test multiplication of 2 and 2"),
        ("http://localhost:8000/add/-1/1", 0, "Test addition of -1 and 1"),
        ("http://localhost:8000/multiply/0/5", 0, "Test multiplication by zero"),
    ]

    @pytest.mark.parametrize("url, expected, description", testcases)
    def test_api(url, expected, description):
        response = requests.get(url)
        result = response.json()["result"]
        assert result == expected, f"{description}. Expected {expected}, got {result}"
    ```
3. **Run the Tests with Pytest**:
    ```bash
    pytest testAutomationPytest.py
    ```

---

### Task 4: Integrating Test Automation with GitHub Actions 🔄
1. **Create GitHub Actions Workflow**: Save the following code as `.github/workflows/test.yml`:
    ```yaml
    name: API Tests

    on:
      push:
        branches:
          - main
      pull_request:
        branches:
          - main

    jobs:
      test:
        runs-on: ubuntu-latest

        steps:
        - name: Checkout code
          uses: actions/checkout@v2

        - name: Set up Python
          uses: actions/setup-python@v2
          with:
            python-version: "3.10"

        - name: Install dependencies
          run: |
            python -m pip install --upgrade pip
            pip install fastapi uvicorn pytest requests

        - name: Start FastAPI server
          run: |
            nohup python apiserver.py &
          env:
            PYTHONUNBUFFERED: 1

        - name: Wait for server to be ready
          run: sleep 5

        - name: Run tests
          run: pytest testAutomationPytest.py
    ```
2. **Commit and Push to GitHub**:
    ```bash
    git add .
    git commit -m "Add test automation and GitHub Actions"
    git push origin main
    ```
3. **View Results**: Check the **Actions** tab in your GitHub repository for test results. 🎉

---

### Task 5: Expanding the Automation 🏗️
1. **Database Integration**: Implement database interactions and test with mocked data. 🗄️
2. **Authentication**: Secure endpoints with OAuth2, JWT, or API keys and add tests for access. 🔐
3. **Advanced Logging**: Set up centralized logging with **CloudWatch** or **ELK Stack**. 📊
4. **Performance Testing**: Use tools like **locust** or **pytest-benchmark** for load testing. 🏃‍♂️

---

## Conclusion 🎯

Through this project, you've learned how to:
- Set up a **FastAPI** backend. 🖥️
- Automate **API tests** using Python. 🧪
- Integrate testing into a **CI/CD pipeline** with **GitHub Actions**. 🔄
- Expand automation for **real-world applications**. 🏗️

This experiment is a stepping stone to mastering backend development and **DevOps** practices. 🚀

---

**Enjoy building and testing!** 🎉
