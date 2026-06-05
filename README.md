### algo_HW3_A_c
עבודת הגשה 3 באלגוריתמים מתקדמים, שאלה 1 סעיף ג

מגישים: יניר לטישב | נדב מייק

-------------------------------------------------------------------------------------------------------------------------------------------------------------------
The code ( was made in Colab ):

`
!pip install pulp
`


import pulp
### הגדרת קיבולות הספקים (a_i)
suppliers = {
    'u1': 3.5,
    'u2': 2,
    'u3': 4,
    'u4': 1.5
}

### הגדרת דרישות הצרכנים (b_j)
consumers = {
    'v1': 2.5,
    'v2': 3,
    'v3': 2,
    'v4': 1.5,
    'v5': 2
}

### הגדרת הקשתות (E) החוקיות בגרף הדו-צדדי
edges = [
    ('u1', 'v1'), ('u1', 'v2'), ('u1', 'v4'),
    ('u2', 'v2'), ('u2', 'v5'),
    ('u3', 'v3'), ('u3', 'v4'),
    ('u4', 'v2'), ('u4', 'v5')
]

### הגדרת הבעיה: אנחנו רוצים למקסם את הסכום
prob = pulp.LpProblem("Maximize_Supply_Consumer_Flow", pulp.LpMaximize)

### הגדרת משתני ההחלטה (x_i,j) - הכמות המועברת מכל ספק לכל צרכן
### המשתנים רציפים וחייבים להיות גדולים או שווים ל-0
x = pulp.LpVariable.dicts("x", edges, lowBound=0, cat='Continuous')

### פונקציית המטרה: מקסום סך כל הכמויות המועברות על פני כל הקשתות
prob += pulp.lpSum([x[edge] for edge in edges]), "Total_Flow"

### אילוץ 1: כל ספק לא יכול לספק יותר מהיכולת המקסימלית שלו
for u in suppliers:
    prob += pulp.lpSum([x[(u, v)] for (u_i, v) in edges if u_i == u]) <= suppliers[u], f"Max_Supply_{u}"

### אילוץ 2: כל צרכן לא יכול לצרוך יותר מהדרישה המקסימלית שלו
for v in consumers:
    prob += pulp.lpSum([x[(u, v_j)] for (u, v_j) in edges if v_j == v]) <= consumers[v], f"Max_Demand_{v}"

### הרצת הפותרן הלינארי
prob.solve()

###### הדפסת התוצאות
print("Status:", pulp.LpStatus[prob.status])
print(f"Optimal Total Flow: {pulp.value(prob.objective)}")
print("-" * 30)
for edge in edges:
    if x[edge].varValue > 0: ### נדפיס רק את המסלולים שבהם עוברת סחורה בפועל
        print(f"Flow from {edge[0]} to {edge[1]}: {x[edge].varValue}")

-------------------------------------------------------------------------------------------------------------------------

Link to gemini chat:

https://gemini.google.com/share/7eab466316fe

-----------------------------------------------------------------------------------------------------------------------
