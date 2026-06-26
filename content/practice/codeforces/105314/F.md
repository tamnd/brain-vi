---
title: "CF 105314F - Hội chứng Ahmad và hoán đổi"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm, có một chuỗi các từ được sắp xếp thành một dòng. Nhiệm vụ là đếm xem có bao nhiêu cặp vị trí $(i, j)$ với $i < j$ thỏa mãn một điều kiện trực quan đơn giản: hai từ ở các vị trí này “mê hoặc Ahmad” nếu chúng…"
date: "2026-06-23T06:17:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105314
codeforces_index: "F"
codeforces_contest_name: "Robbing Balloons 2.0 Qualifications"
rating: 0
weight: 105314
solve_time_s: 50
verified: true
draft: false
---

[CF 105314F - Ahmad và Hội chứng hoán đổi](https://codeforces.com/problemset/problem/105314/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm, có một chuỗi các từ được sắp xếp thành một dòng. Nhiệm vụ là đếm xem có bao nhiêu cặp vị trí$(i, j)$với$i < j$thỏa mãn một điều kiện trực quan đơn giản: hai từ ở các vị trí này “thu hút Ahmad” nếu chúng bắt đầu bằng cùng một chữ cái. 

Vì vậy, đối với mỗi trường hợp thử nghiệm, về cơ bản chúng tôi đang quét danh sách các chuỗi và đếm xem có bao nhiêu cặp có cùng ký tự đầu tiên. 

Các ràng buộc cho phép lên đến$10^5$Tổng số từ cho mỗi trường hợp kiểm tra trong tất cả các bài kiểm tra và mỗi từ rất ngắn, độ dài tối đa là 13. Điều này có nghĩa là bất kỳ giải pháp nào so sánh trực tiếp từng cặp từ sẽ quá chậm trong trường hợp xấu nhất vì nó sẽ yêu cầu khoảng$n^2$so sánh cho mỗi trường hợp thử nghiệm, khoảng$10^{10}$hoạt động khi$n = 10^5$. Điều đó vượt xa những gì có thể chạy trong một giây. 

Trường hợp cạnh tinh tế là khi tất cả các từ bắt đầu bằng cùng một ký tự. Ví dụ: nếu đầu vào là:```
1
5
aaa aab aac aad aae
```Mọi cặp đều hợp lệ nên câu trả lời là$\binom{5}{2} = 10$. Một giải pháp đơn giản vẫn có thể hoạt động hợp lý nhưng phải tránh tính hai lần hoặc thiếu cặp do logic lặp cặp không chính xác. 

Một trường hợp khác là khi tất cả các từ bắt đầu bằng các chữ cái riêng biệt:```
1
4
apple banana cat dog
```Ở đây câu trả lời là 0 và bất kỳ logic nhóm nào cũng phải xử lý chính xác các nhóm đơn lẻ. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi kiểm tra từng cặp chỉ số$(i, j)$, so sánh ký tự đầu tiên của$w_i$Và$w_j$và tăng câu trả lời nếu chúng khớp. Điều này đúng vì nó đánh giá trực tiếp điều kiện nêu trong bài toán. 

Vấn đề là quy mô. Vì$n = 10^5$, số lần so sánh là khoảng$5 \times 10^9$và ngay cả đối với đầu vào nhỏ hơn nhưng vẫn lớn, nó trở nên quá chậm. 

Điều quan trọng là chỉ có ký tự đầu tiên của mỗi từ mới quan trọng. Thay vì so sánh các từ theo cặp, chúng ta có thể nhóm các từ theo chữ cái bắt đầu của chúng. Nếu một chữ cái xuất hiện$k$lần làm ký tự đầu tiên thì nó đóng góp chính xác$\frac{k(k-1)}{2}$cặp hợp lệ. Điều này chuyển đổi vấn đề từ liệt kê cặp sang đếm tần số. 

Chúng tôi giảm vấn đề từ so sánh bậc hai sang quét tuyến tính đơn lẻ cộng với thao tác liên tục trên 26 chữ cái có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Đếm tần số |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng tần số có kích thước từ 26 đến 0, một khe cho mỗi chữ cái viết thường. Mảng này sẽ lưu trữ bao nhiêu từ bắt đầu bằng mỗi chữ cái. 
2. Đối với mỗi từ trong đầu vào, hãy đọc ký tự đầu tiên của nó và tăng bộ đếm tần số tương ứng. Bước này nén tất cả thông tin liên quan từ chuỗi thành một bản cập nhật số nguyên duy nhất. 
3. Sau khi xử lý tất cả các từ, lặp lại 26 chữ cái. Với mỗi tần số$f$, tính số cặp hợp lệ được đóng góp bởi chữ cái đó bằng công thức$f \times (f - 1) / 2$, và thêm nó vào câu trả lời. 
4. Xuất tổng tích lũy cho test case. 

### Tại sao nó hoạt động 

Thuật toán dựa vào việc phân chia các chỉ mục thành các nhóm rời rạc dựa trên ký tự đầu tiên. Mỗi cặp hợp lệ phải đến từ chính xác một nhóm như vậy và mọi cặp trong nhóm đều hợp lệ. Vì các nhóm không trùng nhau nên các kết hợp tổng bên trong mỗi nhóm sẽ tính mỗi cặp hợp lệ chính xác một lần và không bao giờ bao gồm cặp không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        words = input().split()
        
        freq = [0] * 26
        
        for w in words:
            freq[ord(w[0]) - 97] += 1
        
        ans = 0
        for f in freq:
            ans += f * (f - 1) // 2
        
        print(ans)

if __name__ == "__main__":
    solve()
```Mã xử lý từng trường hợp thử nghiệm một cách độc lập. Chi tiết triển khai chính là chỉ ký tự đầu tiên được truy cập, vì vậy chúng tôi tránh được việc xử lý chuỗi không cần thiết. Mảng tần số được đặt lại cho mỗi trường hợp thử nghiệm để tránh rò rỉ giữa các trường hợp. 

Công thức đếm cặp được áp dụng sau khi đọc tất cả thông tin đầu vào, điều này đảm bảo chúng ta không vô tình đếm một phần thông tin. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
5
apple ant axe bat ball
```Chúng tôi theo dõi tần suất bằng chữ cái đầu tiên: 

| Bước | Lời | Char đầu tiên | Mảng tần số (một phần) | 
| --- | --- | --- | --- | 
| 1 | táo | một | a=1 | 
| 2 | kiến | một | a=2 | 
| 3 | rìu | một | a=3 | 
| 4 | dơi | b | a=3, b=1 | 
| 5 | bóng | b | a=3, b=2 | 

Bây giờ hãy tính các cặp: 

- cho 'a':$3 \cdot 2 / 2 = 3$- cho 'b':$2 \cdot 1 / 2 = 1$Tổng cộng = 4. 

Điều này cho thấy cách nhóm chuyển đổi việc đếm cặp thành tổ hợp đơn giản. 

### Ví dụ 2 

đầu vào:```
1
4
cat dog fish goat
```| Bước | Lời | Char đầu tiên | Mảng tần số | 
| --- | --- | --- | --- | 
| 1 | mèo | c | c=1 | 
| 2 | con chó | d | c=1, d=1 | 
| 3 | cá | f | c=1, d=1, f=1 | 
| 4 | dê | g | c=1, d=1, f=1, g=1 | 

Tất cả các tần số đều bằng 1, vì vậy mọi đóng góp đều bằng 0. 

Điều này xác nhận rằng các nhóm đơn lẻ không tạo ra cặp nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi từ được xử lý một lần và chỉ ký tự đầu tiên của nó được sử dụng | 
| Không gian |$O(1)$| Mảng tần số có kích thước cố định 26 bất kể đầu vào | 

Giải pháp dễ dàng nằm trong giới hạn vì tổng$n \le 10^5$, làm cho công việc trở nên tuyến tính về tổng thể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        words = input().split()
        freq = [0] * 26
        for w in words:
            freq[ord(w[0]) - 97] += 1
        ans = 0
        for f in freq:
            ans += f * (f - 1) // 2
        out.append(str(ans))
    return "\n".join(out)

# provided samples (illustrative placeholders)
assert run("1\n3\napple ant axe\n") == "3"
assert run("1\n4\ncat dog fish goat\n") == "0"

# custom cases
assert run("1\n1\na\n") == "0", "single word"
assert run("1\n2\na ab\n") == "1", "two same prefix"
assert run("1\n5\na aa aaa aaaa aaaaa\n") == "10", "all same prefix"
assert run("1\n6\na b c d e f\n") == "0", "all distinct"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| từ đơn | 0 | không có cặp nào tồn tại | 
| hai tiền tố giống nhau | 1 | đếm cặp cơ bản | 
| tất cả cùng một tiền tố | 10 | tính đúng đắn của tổ hợp | 
| tất cả đều khác biệt | 0 | không có kết quả dương tính giả | 

## Vỏ cạnh 

Đối với trường hợp kiểm tra một từ như`1 a`, mảng tần số chứa một mục khác 0 là 1. Công thức kết hợp cho$1 \cdot 0 / 2 = 0$, do đó kết quả đầu ra là chính xác. 

Đối với trường hợp tất cả các từ đều có cùng chữ cái đầu tiên, chẳng hạn như`a aa aaa aaaa`, tần số trở thành 4 cho 'a'. Thuật toán tính toán$4 \cdot 3 / 2 = 6$, khớp với số lượng các cặp chỉ số không có thứ tự. 

Đối với một bộ hoàn toàn đa dạng như`a b c d e`, mỗi tần số là 1, do đó tất cả các đóng góp đều bằng 0 và đầu ra vẫn bằng 0, tránh việc ghép cặp chữ cái ngẫu nhiên một cách chính xác.
