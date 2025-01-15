# Learning notes

## JWT Pizza code study and debugging
 
As part of `Deliverable ⓵ Development deployment: JWT Pizza`, start up the application and debug through the code until you understand how it works. During the learning process fill out the following required pieces of information in order to demonstrate that you have successfully completed the deliverable.


 
|**User activity**|**Frontend component**|**Backend endpoints**|**Database SQL**|
|------------------|-----------------------|----------------------|------------------------------------------------------------------------------------------------|
|View home page|home.tsx|none|none|
|Register new user<br/>(t@jwt.com, pw: test)|register.tsx|[POST] /api/auth|INSERT INTO user (name, email, password, created_at) VALUES (?, ?, ?, NOW());<br>INSERT INTO userRole (userId, role, objectId) VALUES (LAST_INSERT_ID(), ?, ?);|
|Login new user<br/>(t@jwt.com, pw: test)|login.tsx|[PUT] /api/auth|SELECT * FROM user WHERE email = ? AND password = ?;<br>SELECT role FROM userRole WHERE userId = ?;|
|Order pizza|payment.tsx|[POST] /api/order|INSERT INTO orders (franchiseId, storeId, totalPrice, created_at) VALUES (?, ?, ?, NOW());<br>INSERT INTO orderItems (orderId, menuId, quantity, price) VALUES (?, ?, ?, ?);|
|Verify pizza|delivery.tsx|[POST] /api/order|SELECT * FROM orders WHERE id = ?;<br>SELECT * FROM orderItems WHERE orderId = ?;|
|View profile page|dinerDashboard.tsx|[GET] /api/order|SELECT * FROM orders WHERE userId = ? ORDER BY created_at DESC;|
|View franchise (as diner)|franchiseDashboard.tsx|[GET] /api/franchise/:userId|SELECT * FROM franchises WHERE userId = ?;<br>SELECT * FROM stores WHERE franchiseId IN (SELECT id FROM franchises WHERE userId = ?);|
|Logout|none|[DELETE] /api/auth|none|
|View about page|about.tsx|none|none|
|View history page|history.tsx|none|none|
|Login as franchisee<br/>(f@jwt.com, pw: franchisee)|login.tsx|[PUT] /api/auth|SELECT * FROM user WHERE email = ? AND password = ?;<br>SELECT role FROM userRole WHERE userId = ?;|
|View franchise (as franchisee)|franchiseDashboard.tsx|[GET] /api/franchise/:userId|SELECT * FROM franchises WHERE userId = ?;<br>SELECT * FROM stores WHERE franchiseId IN (SELECT id FROM franchises WHERE userId = ?);|
|Create a store|createStore.tsx|[POST] /api/franchise/:franchiseId/store|INSERT INTO stores (franchiseId, name, created_at) VALUES (?, ?, NOW());|
|Close a store|closeStore.tsx|[DELETE] /api/franchise/:franchiseId/store/:storeId|DELETE FROM stores WHERE id = ? AND franchiseId = ?;|
|Login as admin<br/>(a@jwt.com, pw: admin)|login.tsx|[PUT] /api/auth|SELECT * FROM user WHERE email = ? AND password = ?;<br>SELECT role FROM userRole WHERE userId = ?;|
|View Admin page|adminDashboard.tsx|[GET] /api/franchise|SELECT * FROM franchises;|
|Create a franchise for t@jwt.com|createFranchise.tsx|[POST] /api/franchise|INSERT INTO franchises (name, created_at) VALUES (?, NOW());<br>INSERT INTO franchiseAdmins (franchiseId, userId) VALUES (LAST_INSERT_ID(), ?);|
|Close the franchise for t@jwt.com|closeFranchise.tsx|[DELETE] /api/franchise/:franchiseId|DELETE FROM franchises WHERE id = ?;<br>DELETE FROM stores WHERE franchiseId = ?;|
