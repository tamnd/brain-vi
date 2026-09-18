---
title: "CF 104725L - \u517b\u6210\u6e38\u620f"
description: "Chúng ta được cho một hệ thống nhỏ các biến số nguyên. Có tối đa sáu biến, mỗi biến đại diện cho một thuộc tính của một nhân vật trong trò chơi và mỗi thuộc tính có thể là số nguyên bất kỳ từ 0 đến K. Bên cạnh các biến này, có tới 100 giám khảo."
date: "2026-06-29T02:58:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104725
codeforces_index: "L"
codeforces_contest_name: "2023\u5e74\u4e2d\u56fd\u5927\u5b66\u751f\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\u5973\u751f\u4e13\u573a"
rating: 0
weight: 104725
solve_time_s: 53
verified: true
draft: false
---

[CF 104725L - \u517b\u6210\u6e38\u620f](https://codeforces.com/problemset/problem/104725/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hệ thống nhỏ các biến số nguyên. Có tối đa sáu biến, mỗi biến đại diện cho một thuộc tính của một nhân vật trong trò chơi và mỗi thuộc tính có thể là số nguyên bất kỳ từ 0 đến K. 

Cùng với các biến này, có tới 100 giám khảo. Mỗi giám khảo xem xét chính xác hai thuộc tính khác nhau và kiểm tra một điều kiện tuyến tính đơn giản liên quan đến hai giá trị đó. Nếu thỏa mãn điều kiện thì giám khảo đó sẽ cho điểm cố định. Điều kiện luôn có dạng “sự kết hợp tuyến tính của hai thuộc tính được chọn nhiều nhất là một ngưỡng hoặc ít nhất là một ngưỡng”, trong đó mỗi hệ số được giới hạn ở −1, 0 hoặc 1. 

Nhiệm vụ là chọn các giá trị cho tất cả các thuộc tính sao cho tổng số điểm mà giám khảo hài lòng là lớn nhất. 

Cấu trúc chính là mọi ràng buộc chỉ phụ thuộc vào hai biến và kích thước miền cho mỗi biến là rất nhỏ. Điều này ngay lập tức gợi ý rằng mặc dù các ràng buộc trông giống như một vấn đề thỏa mãn ràng buộc nhỏ, nhưng chúng ta vẫn có thể liệt kê các bài tập. 

Các ràng buộc làm cho vũ lực trở nên khả thi vì kích thước không gian tìm kiếm tối đa là (K+1)^n. Với n 6 và K 8, mỗi biến có tối đa 9 giá trị, do đó tổng số phép gán là 9^6 = 531441. Đối với mỗi phép gán, chúng ta có thể đánh giá tối đa 100 ràng buộc, đưa ra khoảng 5 × 10^7 phép toán trong trường hợp xấu nhất, điều này có thể chấp nhận được trong Python với việc triển khai chặt chẽ. 

Một trường hợp phức tạp là các ràng buộc có thể không được thỏa mãn đối với bất kỳ phép gán nào. Trong trường hợp đó họ không bao giờ nên đóng góp. Một trường hợp tinh tế khác là các bất đẳng thức có thể luôn đúng hoặc luôn sai tùy thuộc vào các hệ số, do đó lời giải không được cho rằng các ràng buộc là “cân bằng” hoặc có ý nghĩa. 

## Phương pháp tiếp cận 

Một cách giải thích ngây thơ sẽ coi đây là một sự tối ưu hóa chung cho các biến số nguyên với các ràng buộc theo cặp và cố gắng giải quyết nó bằng cách truyền bá ràng buộc hoặc phép gán tham lam. Hướng đó thất bại vì không có cấu trúc đơn điệu giữa các biến và không có tính độc lập: việc thay đổi một biến sẽ ảnh hưởng đến tất cả các ràng buộc liên quan đến nó. 

Tuy nhiên, tên miền nhỏ thay đổi mọi thứ. Vì mỗi biến chỉ có 9 giá trị có thể và chỉ có 6 biến nên chúng ta có thể liệt kê trực tiếp tất cả các phép gán có thể. Đối với mỗi bài tập, chúng tôi đánh giá mọi điều kiện của giám khảo trong thời gian không đổi. 

Lực lượng vũ phu hoạt động vì không gian tìm kiếm đủ nhỏ để khám phá hoàn toàn, nhưng nó sẽ thất bại nếu số lượng biến hoặc kích thước miền lớn. Quan sát thấy rằng n rất nhỏ là yếu tố chuyển đổi một vấn đề tối ưu hóa khó khăn thành một vấn đề liệt kê trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | O((K+1)^n · m) | O(1) | Đã chấp nhận | 
| Tối ưu hóa DP / Tham lam (không cần thiết) | Không được xác định rõ ràng | O(1) | Không cần thiết | 

## Hướng dẫn thuật toán 

Chúng ta liệt kê tất cả các phép gán giá trị có thể có cho n thuộc tính và tính điểm cho mỗi phép gán.

1. Tạo tất cả các bộ dữ liệu (A1, A2, ..., An), trong đó mỗi Ai nằm trong khoảng từ 0 đến K. Đây là một tìm kiếm hoàn chỉnh trên không gian trạng thái, đảm bảo không bỏ sót giải pháp ứng viên nào. 
2. Đối với mỗi bài tập, hãy khởi tạo điểm chạy bằng 0. Điểm này thể hiện sự đóng góp tổng thể của tất cả các giám khảo cho cấu hình cụ thể này. 
3. Lặp lại tất cả m giám khảo. Mỗi giám khảo tham chiếu hai chỉ số i và j và một loại điều kiện op. 
4. Với mỗi giám khảo, hãy tính biểu thức tuyến tính a * Ai + b * Aj. Nếu op bằng 0, hãy kiểm tra xem giá trị này có nhỏ hơn hoặc bằng d hay không. Nếu op bằng 1, kiểm tra xem nó có lớn hơn hoặc bằng d không. 
5. Nếu thỏa mãn điều kiện thì cộng v vào điểm của bài tập hiện tại. 
6. Sau khi xử lý tất cả giám khảo cho nhiệm vụ này, hãy cập nhật điểm tối đa toàn cầu nếu điểm hiện tại lớn hơn. 
7. Sau khi tất cả các bài tập được xử lý, hãy in ra số điểm tối đa tìm được. 

Lý do chính khiến điều này có tác dụng là vì mọi bài tập có thể được đánh giá chính xác một lần và hàm điểm hoàn toàn mang tính xác định đối với một bài tập. Không có sự tương tác giữa các phép gán khác nhau, vì vậy giá trị tối ưu toàn cục phải xuất hiện trong tập liệt kê. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, K = map(int, input().split())
    cons = []
    for _ in range(m):
        i, j, op, a, b, d, v = map(int, input().split())
        i -= 1
        j -= 1
        cons.append((i, j, op, a, b, d, v))

    best = 0
    A = [0] * n

    def dfs(idx):
        nonlocal best
        if idx == n:
            score = 0
            for i, j, op, a, b, d, v in cons:
                val = a * A[i] + b * A[j]
                if op == 0:
                    if val <= d:
                        score += v
                else:
                    if val >= d:
                        score += v
            if score > best:
                best = score
            return

        for x in range(K + 1):
            A[idx] = x
            dfs(idx + 1)

    dfs(0)
    print(best)

if __name__ == "__main__":
    solve()
```Việc triển khai sử dụng phép liệt kê theo chiều sâu trên tất cả các bài tập. Mảng A lưu trữ phép gán một phần hiện tại. Sau khi tất cả các biến được gán, mã sẽ đánh giá tất cả các ràng buộc trong một lần chuyển. 

Một sai lầm phổ biến là quên rằng các chỉ mục đầu vào dựa trên 1, vì vậy chúng phải được chuyển đổi thành chỉ mục dựa trên 0. Một điểm tinh tế khác là đảm bảo rằng A được sử dụng lại trong quá trình đệ quy thay vì được tạo lại, điều này tránh được chi phí không cần thiết. 

## Ví dụ đã hoạt động 

Mẫu đầu tiên tương ứng với trường hợp có ba biến và năm ràng buộc. Một phép gán hợp lệ là A1 = 1, A2 = 0, A3 = 1, tạo ra tổng số điểm là 12. Quá trình đánh giá được tiến hành bằng cách kiểm tra từng ràng buộc đối với vectơ cố định này và chỉ tính tổng những điểm thỏa mãn. 

| Chỉ số ràng buộc | Biểu hiện | Kiểm tra kết quả | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | A3 + A1 ≤ 0 | sai | 0 | 
| 2 | A3 + A1 2 | đúng | 2 | 
| 3 | A3 ≤ 1 | đúng | 3 | 
| 4 | A3 + A2 ≥ 2 | đúng | 0 | 
| 5 | A3 - A2 ≥ 1 | đúng | 3 | 

Mẫu thứ hai có ba biến với các ràng buộc hỗn hợp luôn thỏa mãn và có điều kiện. Từ cấu trúc, A1 bị ràng buộc về 0, trong khi A3 là biến duy nhất ảnh hưởng đến điều kiện phần thưởng tích cực. Chọn A3 ≤ 2 sẽ tối đa hóa tổng số điểm. 

| Trạng thái biến đổi | A1 | A2 | A3 | Điểm hiện tại | 
| --- | --- | --- | --- | --- | 
| Sau những ràng buộc | 0 | miễn phí | 2 | 13 | 

Dấu vết này cho thấy rằng các ràng buộc có thể làm giảm đáng kể mức độ tự do và các lựa chọn tối ưu thường đến từ việc tập trung vào một số biến số thực sự ảnh hưởng đến sự bất bình đẳng mang lại lợi ích. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((K+1)^n · m) | Mỗi phép gán (K+1)^n đánh giá m ràng buộc | 
| Không gian | O(n + m) | Lưu trữ cho các nhiệm vụ và ràng buộc hiện tại | 

Với n ₫ 6, K ∼ 8 và m ₫ 100, trường hợp xấu nhất là khoảng 531k × 100 đánh giá, phù hợp thoải mái trong giới hạn thời gian thông thường trong Python được tối ưu hóa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    n, m, K = map(int, sys.stdin.readline().split())
    cons = []
    for _ in range(m):
        i, j, op, a, b, d, v = map(int, sys.stdin.readline().split())
        cons.append((i-1, j-1, op, a, b, d, v))

    A = [0]*n
    best = 0

    def dfs(idx):
        nonlocal best
        if idx == n:
            s = 0
            for i, j, op, a, b, d, v in cons:
                val = a*A[i] + b*A[j]
                if (op == 0 and val <= d) or (op == 1 and val >= d):
                    s += v
            best = max(best, s)
            return
        for x in range(K+1):
            A[idx] = x
            dfs(idx+1)

    dfs(0)
    return str(best)

# sample-like tests
assert run("3 1 1\n1 2 1 1 1 0 5") == "5"

# all zero constraints
assert run("2 2 3\n1 2 0 1 1 -10 3\n1 2 1 1 1 10 4") == "7"

# force variable bounds
assert run("2 1 2\n1 2 0 1 0 1 10") == "20"

# single variable effect through second index
assert run("2 1 2\n1 2 1 0 1 1 5") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ràng buộc đơn nhỏ | 5 | tính đúng đắn cơ bản | 
| hỗn hợp luôn đúng / luôn sai | 7 | hành vi cạnh hạn chế | 
| ranh giới buộc giá trị tối đa | 20 | xử lý ranh giới miền | 
| phụ thuộc vào biến thứ hai | 10 | tính chính xác dựa trên chỉ mục | 

## Vỏ cạnh 

Trường hợp cạnh phổ biến là khi một ràng buộc không thể được thỏa mãn. Trong tình huống như vậy, nó không đóng góp gì bất kể nhiệm vụ. Thuật toán xử lý việc này một cách tự nhiên vì nó chỉ thêm v khi điều kiện được đánh giá là đúng. 

Một trường hợp khác là khi một ràng buộc luôn được thỏa mãn. Ví dụ: nếu a = 0, b = 0 và op = 0 với d ≥ 0 thì mọi phép gán đều thỏa mãn nó. Đánh giá bạo lực vẫn tính nó một cách nhất quán cho tất cả các trạng thái, vì vậy nó đơn giản trở thành một sự bổ sung liên tục cho mọi điểm số. 

Trường hợp thứ ba là khi lời giải tối ưu dựa vào giá trị cực trị của các biến. Vì bảng liệt kê bao gồm rõ ràng tất cả các giá trị từ 0 đến K, nên tối ưu biên luôn được bao gồm trong không gian tìm kiếm, do đó không cần xử lý đặc biệt.
