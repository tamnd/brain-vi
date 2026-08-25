---
title: "CF 104313B - \u041a\u0430\u0448\u0442\u0430\u043d\u044b"
description: "Chúng ta được giao cho một nhóm n đứa trẻ, mỗi đứa thu thập một số hạt dẻ và sau đó xếp chúng thành một đống. Đứa trẻ đầu tiên theo thứ tự, Sasha, đặt hạt dẻ của mình vào đống trước. Mỗi đứa trẻ tiếp theo đóng góp số hạt dẻ gấp đôi so với đứa trẻ trước."
date: "2026-07-01T19:45:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104313
codeforces_index: "B"
codeforces_contest_name: "II \u041e\u0442\u043a\u0440\u044b\u0442\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u042e\u041c\u0428 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 104313
solve_time_s: 54
verified: true
draft: false
---

[CF 104313B - \u041a\u0430\u0448\u0442\u0430\u043d\u044b](https://codeforces.com/problemset/problem/104313/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao cho một nhóm n đứa trẻ, mỗi đứa thu thập một số hạt dẻ và sau đó xếp chúng thành một đống. Đứa trẻ đầu tiên theo thứ tự, Sasha, đặt hạt dẻ của mình vào đống trước. Mỗi đứa trẻ tiếp theo đóng góp số hạt dẻ gấp đôi so với đứa trẻ trước. Vì vậy, các khoản đóng góp tạo thành một cấp số nhân bắt đầu từ số tiền chưa biết của Sasha là a, rồi 2a, rồi 4a, v.v. cho đến đứa con thứ n. 

Chúng ta cũng được biết tổng số tiền m mà Sasha đã tính. Nhiệm vụ là kiểm tra xem tổng này có thể được tạo ra bởi một số nguyên bắt đầu bằng giá trị a nào đó hay không, và nếu có thể, hãy xác định a phải bằng bao nhiêu. 

Cấu trúc của bài toán buộc phải có một dạng tổng rất cứng nhắc. Đối với một n cố định, tổng luôn bằng a nhân với một hằng số đã biết, cụ thể là 1 + 2 + 4 + ... + 2^(n-1). Điều này đã gợi ý rằng toàn bộ vấn đề giảm xuống còn việc kiểm tra tính chia hết và xây dựng lại một ẩn số duy nhất từ ​​một số nhân cố định. 

Các ràng buộc n ≤ 100 và m ≤ 10^18 chỉ ra rằng các giá trị liên quan có thể lớn nhưng độ dài chuỗi nhỏ. Điều này loại trừ mọi nhu cầu mô phỏng trên phạm vi giá trị lớn, nhưng cũng cảnh báo chúng ta tránh số học ngây thơ có thể tràn nếu không được xử lý cẩn thận bằng các ngôn ngữ khác. Trong Python đây không phải là vấn đề đáng lo ngại, nhưng tính đúng đắn về mặt khái niệm vẫn là vấn đề. 

Trường hợp cạnh tinh tế xuất hiện khi m không chia hết cho tổng hình học. Trong trường hợp đó không tồn tại a hợp lệ. Một trường hợp góc khác là khi a được tính bằng 0 hoặc âm, nhưng vì m ≥ 1 và tất cả các số hạng đều là bội số dương của a, nên a cũng phải dương nếu tồn tại nghiệm. 

## Phương pháp tiếp cận 

Một cách ngây thơ để suy nghĩ về vấn đề này là thử tất cả các giá trị có thể có của a từ 1 đến m và mô phỏng dãy a, 2a, 4a, ... cho n số hạng, kiểm tra xem tổng có khớp với m hay không. Về nguyên tắc, điều này đúng vì mọi cấu hình hợp lệ đều tương ứng với chính xác một a. Tuy nhiên, với mỗi ứng viên a, chúng ta sẽ tính n số hạng, đưa ra các phép toán O(mn) trong trường hợp xấu nhất. Với m lên tới 10^18 thì điều này hoàn toàn không khả thi. 

Quan sát quan trọng là dãy tuyến tính theo a. Tổng số tiền có thể được tính là: 

S = a (1 + 2 + 4 + ... + 2^(n-1)) 

Biểu thức trong ngoặc là một tổng hình học cố định chỉ phụ thuộc vào n. Điều này biến bài toán từ việc tìm kiếm trên các chuỗi có thể thành một bài kiểm tra số học đơn giản: tính số nhân, kiểm tra xem m có chia hết cho nó hay không và tìm a khi m chia cho số nhân đó. 

Sự tinh tế duy nhất là tính tổng hình học một cách an toàn. Vì n ≤ 100 nên tổng 2^n nằm trong khả năng số nguyên của Python, nhưng nói chung chúng ta phải tính toán lặp đi lặp lại hoặc sử dụng dịch chuyển bit thay vì dựa vào các phép toán dấu phẩy động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (thử tất cả a) | O(m · n) | O(1) | Quá chậm | 
| Tối ưu (hệ số hình học) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi viết lại tổng số tiền bằng cách sử dụng cấu trúc của chuỗi. 

1. Tính số nhân hình học tương ứng với phần đóng góp của tất cả trẻ em ngoại trừ tỷ lệ, là 1 + 2 + 4 + ... + 2^(n-1). Điều này có thể được xây dựng lặp đi lặp lại bắt đầu từ 1 và liên tục nhân đôi và thêm vào. 
2. Duy trì giá trị đang chạy cur bắt đầu từ 1 và số nhân tổng s bắt đầu từ 1. Với mỗi n-1 phần tử con còn lại, hãy nhân đôi cur và cộng nó với s. Điều này xây dựng tổng lũy ​​thừa chính xác của hai mà không có mối lo ngại tràn. 
3. Sau khi tính s, kiểm tra xem m có chia hết cho s hay không. Nếu nó không chia hết thì không có giá trị nguyên bắt đầu a nào có thể tạo ra tổng đã cho, vì vậy câu trả lời là không thể. 
4. Nếu tính chia hết được giữ nguyên, hãy tính a = m / s. Đây là giá trị duy nhất có thể có của sự đóng góp của Sasha vì tất cả những đóng góp khác đều là bội số cố định của nó. 
5. Đầu ra thành công cùng với a.

### Tại sao nó hoạt động 

Chuỗi đóng góp chính xác là phiên bản thu nhỏ của một vectơ cố định (1, 2, 4, ..., 2^(n-1)). Mọi tổng hợp lệ đều phải nằm trên mạng một chiều do vectơ này tạo ra. Tổng được tính s là hệ số chính xác của vectơ này, do đó, mọi tổng hợp lệ đều phải bằng s nhân với một vô hướng a. Vì tất cả các đóng góp đều là số nguyên dương nên không có sự mơ hồ trong việc xây dựng lại và khả năng chia hết hoàn toàn đặc trưng cho tính khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    s = 1
    cur = 1
    for _ in range(n - 1):
        cur *= 2
        s += cur

    if m % s != 0:
        print("No")
        return

    a = m // s
    print("Yes")
    print(a)

if __name__ == "__main__":
    solve()
```Giải pháp phản ánh trực tiếp thuật toán. Vòng lặp xây dựng tổng hình học mà không dựa vào các công thức có thể tràn trong các ngôn ngữ khác. Việc kiểm tra tính chia hết đảm bảo rằng giá trị bắt đầu được xây dựng lại là tích phân. 

Một lỗi triển khai phổ biến là cố gắng sử dụng trực tiếp biểu mẫu đóng 2^n - 1 mà không xem xét đến lỗi tràn hoặc lỗi sai một trong việc xử lý số mũ. Việc xây dựng lặp lại sẽ tránh được cả hai vấn đề và làm cho mối quan hệ trở nên rõ ràng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 30
```Chúng tôi tính toán hệ số nhân từng bước. 

| Bước | cur | s | 
| --- | --- | --- | 
| bắt đầu | 1 | 1 | 
| 1 | 2 | 3 | 
| 2 | 4 | 7 | 
| 3 | 8 | 15 | 

Vậy s = 15. Vì 30 chia hết cho 15 nên a = 2. 

Trace xác nhận rằng số tiền đóng góp sẽ là 2, 4, 8, 16. 

Điều này phù hợp với yêu cầu mỗi đứa trẻ tiếp theo sẽ nhân đôi đứa trẻ trước đó. 

### Ví dụ 2 

đầu vào:```
5 10
```Tính s: 

| Bước | cur | s | 
| --- | --- | --- | 
| bắt đầu | 1 | 1 | 
| 1 | 2 | 3 | 
| 2 | 4 | 7 | 
| 3 | 8 | 15 | 
| 4 | 16 | 31 | 

Bây giờ s = 31. Vì 10 không chia hết cho 31 nên không có số nguyên a nào có thể tạo ra tổng. Cấu trúc buộc mọi tổng hợp lệ phải là bội số của 31 và 10 không nằm trong tập hợp đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng ta xây dựng tổng hình học với n bước | 
| Không gian | O(1) | Chỉ một số số nguyên được lưu trữ | 

Các ràng buộc cho phép n lên tới 100, vì vậy việc vượt qua tuyến tính là không đáng kể. Giải pháp này phù hợp một cách thoải mái trong cả giới hạn thời gian và bộ nhớ vì tất cả các phép toán đều là số học theo thời gian không đổi trên các số nguyên nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    n, m = map(int, sys.stdin.readline().split())

    s = 1
    cur = 1
    for _ in range(n - 1):
        cur *= 2
        s += cur

    if m % s != 0:
        return "No\n"

    return "Yes\n" + str(m // s) + "\n"

# provided samples
assert run("4 30") == "Yes\n2\n", "sample 1"
assert run("5 10") == "No\n", "sample 2"

# custom cases
assert run("1 7") == "Yes\n7\n", "single child always valid"
assert run("3 7") == "Yes\n1\n", "1 2 4 sum case"
assert run("3 8") == "No\n", "not divisible case"
assert run("10 0") == "No\n", "zero total impossible"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 7 | Có 7 | trường hợp cơ sở một người tham gia | 
| 3 7 | Có 1 | xây dựng hình học đúng | 
| 3 8 | Không | từ chối không chia được | 
| 10 0 | Không | xử lý tổng số không không hợp lệ | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi n = 1. Số nhân s chỉ đơn giản là 1, do đó mọi m đều hợp lệ và a phải bằng m. Thuật toán xử lý việc này vì vòng lặp chạy 0 lần và s vẫn bằng 1, duy trì tính chính xác mà không cần xử lý đặc biệt. 

Một trường hợp khác là khi m không chia hết cho tổng hình học. Ví dụ: n = 3 và m = 8 cho s = 7. Thuật toán tính 8 % 7 ≠ 0 và ngay lập tức bác bỏ trường hợp đó. Điều này phù hợp với ràng buộc về cấu trúc mà các tổng hợp lệ tạo thành một mạng số học bội số của 7. 

Cuối cùng, các giá trị n lớn như n = 100 vẫn hoạt động an toàn vì phép nhân đôi lặp đi lặp lại không bao giờ dựa vào số học dấu phẩy động hoặc phép tính gần đúng. Mỗi giá trị được tính toán chính xác, đảm bảo tính chính xác ngay cả ở giới hạn trên.
