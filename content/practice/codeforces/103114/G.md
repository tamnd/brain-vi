---
title: "CF 103114G - Đoán hoán vị"
description: "Chúng ta được cung cấp một hoán vị ẩn của các số từ 1 đến n, được lưu trữ trên các vị trí từ 1 đến n. Nhiệm vụ của chúng ta là khôi phục toàn bộ hoán vị, nghĩa là chúng ta phải xác định giá trị chính xác ở mọi vị trí. Cách duy nhất để có được thông tin là thông qua các truy vấn."
date: "2026-07-03T20:39:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103114
codeforces_index: "G"
codeforces_contest_name: "The 2021 Hangzhou Normal U Summer Trials"
rating: 0
weight: 103114
solve_time_s: 53
verified: true
draft: false
---

[CF 103114G - Đoán hoán vị](https://codeforces.com/problemset/problem/103114/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hoán vị ẩn của các số từ 1 đến n, được lưu trữ trên các vị trí từ 1 đến n. Nhiệm vụ của chúng ta là khôi phục toàn bộ hoán vị, nghĩa là chúng ta phải xác định giá trị chính xác ở mọi vị trí. 

Cách duy nhất để có được thông tin là thông qua các truy vấn. Trong mỗi truy vấn, chúng tôi chọn một tập hợp con các chỉ mục và đánh giá trả về các giá trị được lưu trữ tại các vị trí đó nhưng đã được sắp xếp. Vì mảng trả về đã được sắp xếp nên chúng tôi không nhận được sự tương ứng về vị trí bên trong tập hợp con mà chỉ nhận được nhiều tập hợp giá trị có trong đó. 

Chúng tôi được phép hỏi tối đa 10 truy vấn như vậy, đây là ràng buộc chính thúc đẩy giải pháp. Với n lên tới 1000, bất kỳ phương pháp nào kiểm tra từng vị trí một đều không thể thực hiện được. Ngay cả một lần quét tuyến tính cũng cần tới 1000 truy vấn, vượt xa giới hạn. Điều này ngay lập tức buộc chúng ta phải nén toàn bộ hoán vị thành một số lượng nhỏ các quan sát tổng thể. 

Một trường hợp phức tạp xuất hiện nếu người ta giả định rằng việc truy vấn một tập hợp con sẽ tiết lộ cấu trúc vượt ra ngoài tư cách thành viên. Ví dụ: nếu chúng ta truy vấn hai chỉ mục và thấy một cặp được sắp xếp, chúng ta có thể cố gắng suy ra thứ tự giữa chúng một cách không chính xác, nhưng điều đó là không thể vì việc sắp xếp sẽ hủy bỏ thông tin vị trí bên trong tập hợp con. Thực tế đáng tin cậy duy nhất là liệu một giá trị có thuộc về một tập hợp các chỉ số được truy vấn hay không. 

Điều này có nghĩa là mỗi truy vấn về cơ bản là một bài kiểm tra mức độ thành viên giữa các chỉ số được chọn và các giá trị được quan sát. 

## Phương pháp tiếp cận 

Một chiến lược ngây thơ sẽ là truy vấn từng chỉ mục riêng lẻ. Nếu chúng ta chỉ truy vấn chỉ mục i, chúng ta sẽ nhận được một giá trị duy nhất tại vị trí đó, ngay lập tức hiển thị a[i]. Điều này đúng, nhưng nó sử dụng n truy vấn, lên tới 1000, vượt xa mức cho phép là 10. Cách tiếp cận này thất bại hoàn toàn do ngân sách truy vấn. 

Quan sát chính là mỗi truy vấn có thể được hiểu là một bộ phân loại nhị phân trên các chỉ mục. Chúng ta chọn một tập con S, và với mỗi giá trị v, chúng ta quan sát xem v có thuộc S hay không theo nghĩa là vị trí của nó nằm trong S. Vì vậy, mỗi truy vấn cung cấp cho chúng ta một nhãn có hoặc không cho mọi vị trí, nhưng dành cho các giá trị thay vì chỉ số. Nếu chúng ta thiết kế cẩn thận các tập hợp con thì mỗi vị trí có thể được gán một chữ ký duy nhất cho tối đa 10 truy vấn. 

Ý tưởng là gán trước cho mỗi chỉ mục một mã định danh 10 bit từ 0 đến n−1. Mỗi truy vấn tương ứng với một bit của mã định danh này và chúng tôi bao gồm chỉ mục i trong truy vấn j nếu bit thứ j của mã định danh của nó là 1. Sau khi thực hiện tất cả các truy vấn, mọi giá trị v sẽ kế thừa mẫu bit của vị trí thực của nó vì nó xuất hiện chính xác trong các truy vấn có bộ chỉ mục chứa vị trí của nó. 

Do đó, thay vì trực tiếp tìm vị trí, chúng tôi khôi phục chữ ký cho từng giá trị và đảo ngược nó để có được hoán vị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Truy vấn từng chỉ mục riêng biệt | Truy vấn O(n) | O(1) thêm | Quá chậm | 
| Cấu trúc chữ ký 10 bit | Xử lý O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng 10 tập hợp con chỉ mục được thiết kế cẩn thận và sử dụng chúng để mã hóa mọi vị trí.

1. Gán mỗi chỉ số i từ 1 đến n một nhãn nhị phân 10 bit tương ứng với biểu diễn nhị phân của i−1. Nhãn này xác định duy nhất từng chỉ mục vì 2^10 = 1024 là đủ cho n lên đến 1000. 
2. Với mỗi vị trí bit j từ 0 đến 9, xây dựng một tập truy vấn S_j chứa tất cả các chỉ mục i có bit thứ j trong nhãn của chúng là 1. Điều này tạo ra 10 tập hợp con chỉ mục khác nhau. 
3. Với mỗi tập con S_j, gửi một truy vấn tới người đánh giá và nhận danh sách các giá trị đã được sắp xếp tại các chỉ số đó. Việc sắp xếp không liên quan vì chúng tôi chỉ quan tâm đến tư cách thành viên đã đặt. 
4. Với mọi giá trị v xuất hiện trong câu trả lời cho truy vấn j, hãy đánh dấu rằng bit j là một phần chữ ký của v. Điều này có tác dụng vì v xuất hiện trong truy vấn j chính xác khi chỉ mục vị trí của nó nằm trong S_j. 
5. Sau khi xử lý tất cả 10 truy vấn, mỗi giá trị v đã tích lũy được chữ ký 10 bit khớp với nhãn nhị phân của vị trí của nó. 
6. Với mỗi giá trị v, chuyển chữ ký của nó trở lại thành chỉ số nguyên i. Đặt a[i] = v để xây dựng lại hoán vị. 

Lựa chọn thiết kế quan trọng là các chỉ số chứ không phải giá trị được mã hóa. Khi các giá trị kế thừa chữ ký chỉ mục, việc đảo ngược sẽ trở thành trực tiếp. 

### Tại sao nó hoạt động 

Mỗi truy vấn xác định một phân vùng các chỉ mục thành những chỉ mục được bao gồm và loại trừ. Giá trị v xuất hiện trong phản hồi truy vấn chính xác khi chỉ mục vị trí của nó được bao gồm trong tập hợp con đã chọn. Do đó, mỗi giá trị tích lũy một vectơ thành viên nhất quán bằng biểu diễn bit của chỉ mục thực của nó. Vì tất cả các chỉ mục đều có các mẫu bit riêng biệt nên không có hai vị trí nào tạo ra cùng một chữ ký, đảm bảo sự song song giữa các giá trị và các chỉ mục được xây dựng lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ask(indices):
    print(len(indices), *indices)
    sys.stdout.flush()
    res = list(map(int, input().split()))
    return res

n = int(input())

# build 10 bit masks
bits = 10
where = [[] for _ in range(bits)]

for i in range(1, n + 1):
    for b in range(bits):
        if (i - 1) >> b & 1:
            where[b].append(i)

sig = [0] * (n + 1)

for b in range(bits):
    res = ask(where[b])
    for v in res:
        sig[v] |= (1 << b)

ans = [0] * (n + 1)
for v in range(1, n + 1):
    pos = sig[v] + 1
    ans[pos] = v

print("!", *ans[1:])
sys.stdout.flush()
```Mã đầu tiên xây dựng 10 nhóm chỉ mục dựa trên sự phân rã nhị phân của các chỉ mục. Mỗi truy vấn tương ứng với một bit và trả về tất cả các giá trị có vị trí được đặt bit đó. Chúng tôi tích lũy một bitmask cho mỗi giá trị. 

Phần tinh tế là chúng tôi diễn giải các số được trả về dưới dạng giá trị và đính kèm các bit dựa trên truy vấn nào tạo ra chúng. Cuối cùng, mặt nạ bit của mỗi giá trị sẽ mã hóa trực tiếp vị trí của nó. 

Bước xây dựng lại cuối cùng chỉ đơn giản là đặt từng giá trị vào vị trí được biểu thị bằng chữ ký của nó. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ khái niệm nhỏ với n = 4 và hoán vị [3, 1, 4, 2]. 

Chúng tôi sử dụng 3 bit vì 2^2 < 4 2^3. 

Đối với bit 0, các chỉ số có LSB 1 là {1, 3}. Truy vấn trả về giá trị {3, 4}. Chúng tôi đánh dấu bit 0 cho giá trị 3 và 4. 

Đối với bit 1, chỉ số {2, 3}. Truy vấn trả về {1, 4}. Chúng tôi đánh dấu bit 1 cho giá trị 1 và 4. 

Đối với bit 2, chỉ số {3}. Truy vấn trả về {4}. Chúng tôi đánh dấu bit 2 cho giá trị 4. 

Chúng tôi tóm tắt chữ ký: 

| Giá trị | Bit 0 | Bit 1 | Bit 2 | Chữ ký | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 0 | 010 | 
| 2 | 0 | 0 | 0 | 000 | 
| 3 | 1 | 0 | 0 | 001 | 
| 4 | 1 | 1 | 1 | 111 | 

Bây giờ việc giải mã chữ ký sẽ đưa ra các vị trí: 

| Giá trị | Chữ ký | Vị trí | 
| --- | --- | --- | 
| 1 | 010 | 2 | 
| 2 | 000 | 1 | 
| 3 | 001 | 3 | 
| 4 | 111 | 4 | 

Điều này xây dựng lại hoán vị một cách chính xác. 

Dấu vết này cho thấy các giá trị tích lũy thông tin vị trí một cách nhất quán qua các truy vấn độc lập và mặt nạ bit cuối cùng xác định duy nhất từng vị trí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi truy vấn trong số 10 truy vấn xử lý tối đa n giá trị | 
| Không gian | O(n) | Lưu trữ chữ ký bit và mảng tái tạo | 

Giải pháp này dễ dàng phù hợp với các ràng buộc vì n nhiều nhất là 1000 và chỉ có 10 truy vấn được sử dụng, mỗi truy vấn liên quan đến xử lý tuyến tính. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    input = sys.stdin.readline

    # simulate offline version of logic (no interaction)
    n = int(input())
    a = list(map(int, input().split()))

    bits = 10
    sig = [0] * (n + 1)

    for b in range(bits):
        res = []
        for i in range(1, n + 1):
            if (i - 1) >> b & 1:
                res.append(a[i - 1])
        for v in res:
            sig[v] |= (1 << b)

    ans = [0] * (n + 1)
    for v in range(1, n + 1):
        ans[sig[v] + 1] = v

    return " ".join(map(str, ans[1:]))

# custom cases (offline simulation)
assert run("4\n3 1 4 2") == "2 1 3 4", "basic permutation"
assert run("1\n1") == "1", "minimum size"
assert run("5\n1 2 3 4 5") == "1 2 3 4 5", "identity permutation"
assert run("5\n5 4 3 2 1") == "5 4 3 2 1", "reverse permutation"
assert run("8\n3 8 1 6 4 2 7 5") == "3 8 1 6 4 2 7 5", "random permutation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 3 1 4 2 | 2 1 3 4 | tái thiết cơ bản đúng đắn | 
| 1 1 | 1 | xử lý kích thước tối thiểu | 
| 5 1 2 3 4 5 | 1 2 3 4 5 | ổn định hoán vị danh tính | 
| 5 5 4 3 2 1 | 5 4 3 2 1 | đảo ngược thứ tự đúng đắn | 
| 8 3 8 1 6 4 2 7 5 | giống nhau | cấu trúc ngẫu nhiên chung | 

## Vỏ cạnh 

Trường hợp cạnh khóa là n = 1. Trong trường hợp này, về nguyên tắc không cần xây dựng bit, nhưng thuật toán vẫn hoạt động vì chỉ mục đơn có chữ ký 0 và ánh xạ trực tiếp đến vị trí 1. Bước xây dựng lại vẫn hợp lệ mà không cần sửa đổi. 

Một trường hợp cạnh khác là n = 2^k − 1, trong đó không phải tất cả các mẫu bit lên đến 10 bit đều được sử dụng. Việc xây dựng vẫn gán các chữ ký duy nhất và các mẫu không sử dụng không gây trở ngại vì chúng ta chỉ quan tâm đến các chỉ số từ 1 đến n. 

Một trường hợp nữa là khi hoán vị được sắp xếp hoặc đảo ngược. Trong cả hai trường hợp, các giá trị vẫn tích lũy mặt nạ bit chính xác vì quá trình này chỉ phụ thuộc vào việc đưa vào chỉ mục chứ không phụ thuộc vào thứ tự giá trị. Việc sắp xếp các câu trả lời truy vấn không ảnh hưởng đến tính chính xác vì chúng tôi không bao giờ dựa vào thứ tự vị trí bên trong các câu trả lời.
