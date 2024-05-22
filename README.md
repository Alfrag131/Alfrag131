    import numpy as np

    def gauss_elimination(A, b):
      n = len(b)
    for i in range(n):
        if A[i][i] == 0.0:
            return None  # Матрица не может быть приведена к треугольному виду
        for j in range(i+1, n):
            ratio = A[j][i]/A[i][i]
            for k in range(n):
                A[j][k] = A[j][k] - ratio * A[i][k]
            b[j] = b[j] - ratio * b[i]
    x = np.zeros(n)
    x[n-1] = b[n-1]/A[n-1][n-1]
    for i in range(n-2,-1,-1):
        x[i] = b[i]
        for j in range(i+1,n):
            x[i] = x[i] - A[i][j]*x[j]
        x[i] = x[i]/A[i][i]
    return x

    def jacobi_method(A, b, x0=None, tol=1e-10, max_iter=1000):
    n = len(b)
    if x0 is None:
        x0 = np.zeros(n)
    x_new = np.copy(x0)
    x_old = np.zeros(n)
    for _ in range(max_iter):
        for i in range(n):
            x_old[i] = x_new[i]
            sigma = 0
            for j in range(n):
                if j != i:
                    sigma += A[i][j] * x_old[j]
            x_new[i] = (b[i] - sigma) / A[i][i]
        if np.linalg.norm(x_new - x_old) < tol:
            return x_new
    return None  # Метод не сошелся

    def cramer_method(A, B):
    det_A = np.linalg.det(A)
    n = A.shape[1] 
    m = B.shape[1]  
    if det_A == 0:
        return None  # Определитель=0
    x = []
    for i in range(n):
        A_copy = np.copy(A)
        A_copy[:, i] = B[:, 0]  # Заменить столбец А на столбец В
        x.append(np.linalg.det(A_copy) / det_A)
    return np.array(x).reshape(n, m)  # Изменить форму x, чтобы она соответствовала форме B

    def integration_method(A, b):
    # Реализация метода интеграции для решения СЛАУ
    pass

    def kroncker_capelli_method(A, b):
    if np.linalg.matrix_rank(A) != np.linalg.matrix_rank(np.hstack((A, b.reshape(-1, 1)))):
        return None  # Система несовместна
    return np.linalg.lstsq(A, b, rcond=None)[0]

    def jacobi_iterative_method(A, b, x0=None, tol=1e-10, max_iter=1000):
    n = len(b)
    if x0 is None:
        x0 = np.zeros(n)
    D = np.diag(np.diag(A))
    LU = A - D
    x_new = np.copy(x0)
    x_old = np.zeros(n)
    for _ in range(max_iter):
        x_old[:] = x_new
        x_new = np.dot(np.linalg.inv(D), b - np.dot(LU, x_old))
        if np.linalg.norm(x_new - x_old) < tol:
            return x_new
    return None  # Метод не сошелся


    def matrix_method(A, b):
    return np.dot(np.linalg.inv(A), b)

    def input_matrix_and_matrix():
    print("Введите матрицу коэффициентов A:")
    A = []
    rows_A = int(input('Количество строк: '))
    cols_A = int(input('Количество столбцов: '))
    print("Введите элементы матрицы A построчно, разделяя их пробелами или запятыми.")
    print("Например, для строки 1 2 3 введите '1 2 3'")
    for i in range(rows_A):
        while True:
            row_input = input(f"Введите элементы {i+1}-й строки: ")
            row = [float(x) for x in row_input.replace(',', ' ').split()]
            if len(row) == cols_A:
                break
            else:
                print(f"Неверное количество элементов. Должно быть {cols_A} элементов.")
        A.append(row)

    print("Введите вторую матрицу B (размерность должна соответствовать):")
    B = []
    rows_B = rows_A  
    cols_B = int(input("Количество столбцов: "))
    print("Введите элементы матрицы B построчно, разделяя их пробелами или запятыми.")
    print("Например, для строки 1 2 3 введите '1 2 3'")
    for i in range(rows_B):
        while True:
            row_input = input(f"Введите элементы {i+1}-й строки: ")
            row = [float(x) for x in row_input.replace(',', ' ').split()]
            if len(row) == cols_B:
                break
            else:
                print(f"Неверное количество элементов. Должно быть {cols_B} элементов.")
        B.append(row)

    return np.array(A), np.array(B)   
    
    # Пример использования:
    print("Введите данные для решения СЛАУ:")
    A, B = input_matrix_and_matrix()
    if A is not None and B is not None:
    # Выбор метода решения
    print("Выберите метод решения СЛАУ (1 - Гаусса, 2 - Зейделя, 3 - Крамера, 4 - Интеграции, 5 - Кронекера-Капелли, 6 - Итерационный метод Якоби, 7 - Матричный метод): ")
    method = input()
    if method == '1':
        solution = gauss_elimination(A, B)
    elif method == '2':
        solution = jacobi_method(A, B)
    elif method == '3':
        solution = cramer_method(A, B)
    elif method == '4':
        solution = integration_method(A, B)
    elif method == '5':
        solution = kroncker_capelli_method(A, B)
    elif method == '6':
        solution = jacobi_iterative_method(A, B)
    elif method == '7':
        solution = matrix_method(A, B)
    else:
        solution = None
        print("Некорректный выбор метода")

    if solution is not None:
        print("Решение:", solution)
    else:
        print("Система не имеет решения или выбран неподдерживаемый метод.")
