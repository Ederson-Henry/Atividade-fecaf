Exemplos de Quality Assurance com Python e GitHub Actions
1. Testes Unitários com unittest

# arquivo: calculadora.py
def soma(a, b):
    return a + b

def divisao(a, b):
    if b == 0:
        raise ValueError("Divisão por zero não é permitida.")
    return a / b
    
# arquivo: test_calculadora.py

import unittest
from calculadora import soma, divisao

class TestCalculadora(unittest.TestCase):
    def test_soma(self):
        self.assertEqual(soma(2, 3), 5)

    def test_divisao(self):
        self.assertEqual(divisao(10, 2), 5)

    def test_divisao_por_zero(self):
        with self.assertRaises(ValueError):
            divisao(10, 0)

if __name__ == '__main__':
    unittest.main()
    
2. Automação com Selenium

# Instalar: pip install selenium
from selenium import webdriver
from selenium.webdriver.common.by import By

driver = webdriver.Chrome()
driver.get("https://www.google.com")

search_box = driver.find_element(By.NAME, "q")
search_box.send_keys("Quality Assurance com Python")
search_box.submit()

print(driver.title)
driver.quit()
    
3. Testes de Carga com Locust

# Instalar: pip install locust
from locust import HttpUser, task

class MeuTesteCarga(HttpUser):
    @task
    def carregar_home(self):
        self.client.get("/")
# Execute com:
# locust -f nome_arquivo.py --host=http://localhost:8000
    
4. Integração com GitHub Actions

Estrutura do Projeto:
meu_projeto/
├── calculadora.py
├── test_calculadora.py
└── .github/
    └── workflows/
        └── python-tests.yml
    
Código: calculadora.py

def soma(a, b):
    return a + b

def subtrai(a, b):
    return a - b
    
Código: test_calculadora.py

import unittest
from calculadora import soma, subtrai

class TestCalculadora(unittest.TestCase):
    def test_soma(self):
        self.assertEqual(soma(1, 2), 3)

    def test_subtrai(self):
        self.assertEqual(subtrai(5, 3), 2)

if __name__ == '__main__':
    unittest.main()
    
Workflow: .github/workflows/python-tests.yml

name: Python Tests

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v4

    - name: Set up Python
      uses: actions/setup-python@v5
      with:
        python-version: '3.10'

    - name: Install dependencies
      run: pip install -r requirements.txt || true

    - name: Run tests
      run: python -m unittest discover
    
