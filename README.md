# node-mongo-signup-verification-api

NodeJS + MongoDB - API with Email Sign Up, Verification, Authentication & Forgot Password

For documentation and instructions see https://jasonwatmore.com/post/2020/05/13/node-mongo-api-with-email-sign-up-verification-authentication-forgot-password

# Cách sử dụng

1. Register: Khi gọi hàm register, hệ thống sẽ sinh ra 1 giá trị token và lưu vào trường verificationToken
2. Verify Email: Sau khi register xong, truyền tham số "token" vào body để verify. Lúc này hệ thống sẽ xoá field verificationToken và update trường verified là ngày hiện tại.
3. Forgot Password: Truyền email vào để lấy pass, hệ thống sẽ sinh ra 1 object: resetToken {token, expires}. Vẫn login bình thường nếu nhớ pass.
4. Reset password: truyền token ở bước 3 + password mới. Hệ thống sẽ remove resetToken đi, và update lại password + update thời gian passwordReset
5. Authentication: Có passwordReset -> Chứng tỏ email đúng; có verified -> Chứng tỏ đã verify
6. RefreshToken: Lấy trong cookie lên giá trị refreshToken để sinh mới, sinh mới cả refresh Token & JWT. NHư vậy có thể có nhiều hơn 2 JWT cùng tồn tại và gọi lên được. Phải blacklist JWT cũ (trong code chưa làm vậy). RefreshToken sẽ được thay thế = refreshToken new & thêm field revoked vào refreshToken cũ.
7. RevokeToken: truyền JWT vào Bearer, và token vào body hoặc lấy từ cookie. Thường nên vô hiệu hoá refresh TOken
8. List Accounts: là admin
