---
title: "CF 105672E - Đào tạo tương tác cổ điển"
description: "Nhiệm vụ ban đầu được thiết kế như một thử thách tương tác trong đó chương trình phải khám phá một hoán vị ẩn bằng cách đặt câu hỏi về vị trí của các giá trị. Trong phiên bản hack được Codeforces sử dụng, hoán vị ẩn không còn bị ẩn nữa."
date: "2026-06-26T09:54:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105672
codeforces_index: "E"
codeforces_contest_name: "TheForces Round #39 (1000-Forces)"
rating: 0
weight: 105672
solve_time_s: 38
verified: true
draft: false
---

[CF 105672E - Đào tạo tương tác cổ điển](https://codeforces.com/problemset/problem/105672/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ ban đầu được thiết kế như một thử thách tương tác trong đó chương trình phải khám phá một hoán vị ẩn bằng cách đặt câu hỏi về vị trí của các giá trị. Trong phiên bản hack được Codeforces sử dụng, hoán vị ẩn không còn bị ẩn nữa. Đầu vào trực tiếp chứa hoán vị và đầu ra được yêu cầu chỉ đơn giản là hoán vị tương tự. 

Hoán vị ở đây là sự sắp xếp các số từ`1`ĐẾN`n`. Đầu vào mô tả một số trường hợp thử nghiệm độc lập. Đối với mỗi trường hợp, chúng tôi nhận được kích thước của hoán vị và sau đó là các giá trị được đặt ở mọi vị trí. Đầu ra sẽ tái tạo hoán vị cho từng trường hợp thử nghiệm. 

Các ràng buộc đủ nhỏ để thậm chí một cách tiếp cận phức tạp hơn cũng có thể vượt qua, nhưng chúng cũng được thiết kế để làm cho quan sát dự định trở nên rõ ràng. Từ`n`nhiều nhất là`100`và có thể có nhiều nhất`20`trường hợp thử nghiệm, tổng số lượng đầu vào là rất nhỏ. Any solution that only reads the data and writes the answer uses a negligible number of operations. There is no need for searching, reconstruction, graph processing, or simulation of the interactive queries.

 The main edge cases are related to correctly preserving the given order.

 Đối với một hoán vị nhỏ nhất có thể:```
Input:
1
3
1 2 3
```đầu ra phải là:```
1 2 3
```Việc triển khai bất cẩn sắp xếp các giá trị trước khi in vẫn có tác dụng với ví dụ này, nhưng nó sẽ thất bại trong hoán vị chung vì thứ tự các vị trí là một phần của câu trả lời. 

Đối với hoán vị ngược:```
Input:
1
5
5 4 3 2 1
```đầu ra phải là:```
5 4 3 2 1
```Việc triển khai giả định hoán vị sẽ tăng lên sẽ in không chính xác`1 2 3 4 5`. 

Đối với các trường hợp thử nghiệm lặp lại:```
Input:
2
3
2 1 3
4
4 1 3 2
```đầu ra phải chứa cả hai hoán vị theo cùng một thứ tự:```
2 1 3
4 1 3 2
```Một chương trình chỉ lưu trữ một trường hợp kiểm thử hoặc quên đặt lại dữ liệu giữa các trường hợp có thể âm thầm tạo ra kết quả không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực từ phiên bản tương tác ban đầu cố gắng khám phá hoán vị bằng cách đặt câu hỏi về vị trí tương đối. Một chương trình sẽ cần phải thực hiện nhiều truy vấn để tìm hiểu xem mỗi giá trị thuộc về đâu. Khó khăn đến từ việc chương trình không nhìn thấy trực tiếp hoán vị. 

Trong phiên bản hack, toàn bộ quá trình đó sẽ biến mất. Đầu vào đã chứa thông tin mà giám khảo tương tác có thể đã ẩn. Cách tiếp cận nhanh nhất chỉ đơn giản là đọc từng hoán vị và in nó không thay đổi. 

Tư duy tương tác vũ phu ở đây là không cần thiết vì không còn thông tin nào chưa biết để phục hồi. Quan sát rằng hoán vị được cung cấp rõ ràng sẽ làm giảm toàn bộ vấn đề đối với việc xử lý đầu vào và đầu ra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) hoặc tệ hơn tùy thuộc vào truy vấn được mô phỏng | O(n) | Không cần thiết | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Mỗi trường hợp thử nghiệm đều độc lập nên trạng thái xử lý luôn bắt đầu mới. 
2. Đọc`n`, kích thước của hoán vị và đọc phần tiếp theo`n`số nguyên. Những số nguyên này đã là câu trả lời cuối cùng cho trường hợp thử nghiệm này. 
3. Lưu trữ các giá trị hoán vị trong bộ đệm đầu ra. Giữ tất cả đầu ra cùng nhau sẽ tránh việc xả nước không cần thiết và làm cho chương trình nhanh hơn. 
4. In mọi hoán vị đã lưu trên dòng riêng của nó. 

Tại sao nó hoạt động: 

Hoán vị đầu vào chính xác là trình tự mà bài toán tương tác ban đầu yêu cầu chúng ta xác định. Vì không cần chuyển đổi nên việc sao chép các giá trị từ đầu vào sang đầu ra sẽ duy trì sự sắp xếp chính xác. Điều bất biến là sau khi đọc một trường hợp thử nghiệm, trình tự được lưu trữ giống hệt với hoán vị ẩn mà phiên bản tương tác sẽ yêu cầu chúng ta xây dựng lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        ans.append(" ".join(map(str, p)))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Phần đầu tiên đọc số lượng test case và chuẩn bị danh sách đầu ra. Việc thu thập câu trả lời và in một lần ở cuối sẽ tránh được các thao tác xuất lặp lại. 

Bên trong vòng lặp, chương trình đọc trực tiếp hoán vị. Biến`n`chỉ được sử dụng để xác định kích thước của trường hợp hiện tại vì dòng tiếp theo đã chứa hoán vị hoàn chỉnh. 

Việc chuyển đổi với`" ".join(map(str, p))`là cần thiết vì đầu ra là văn bản. Thứ tự của các phần tử không bao giờ thay đổi, đó là chi tiết chính của vấn đề này. 

Không có tính toán ranh giới, điều chỉnh chỉ mục hoặc phép tính số học, do đó, các lỗi phổ biến như lỗi sai một hoặc tràn số nguyên không thể xảy ra. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào này:```
2
3
2 3 1
4
4 3 2 1
```Dấu vết là: 

| Bước | Trường hợp thử nghiệm | n | Đọc hoán vị | Dòng đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | [2, 3, 1] | 2 3 1 | 
| 2 | 2 | 4 | [4, 3, 2, 1] | 4 3 2 1 | 

Trường hợp đầu tiên chứng minh rằng các giá trị được sao chép mà không cần sắp xếp hoặc sửa đổi. Trường hợp thứ hai xác nhận rằng hoán vị giảm dần được xử lý chính xác như mọi đầu vào khác. 

Một ví dụ thứ hai:```
1
6
6 1 5 2 4 3
```| Bước | Trường hợp thử nghiệm | n | Đọc hoán vị | Dòng đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 6 | [6, 1, 5, 2, 4, 3] | 6 1 5 2 4 3 | 

Trường hợp này thực hiện một thứ tự có vẻ ngẫu nhiên. Thuật toán thành công vì nó không cố gắng suy ra một mẫu không tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi giá trị trong hoán vị được đọc một lần và viết một lần. | 
| Không gian | O(n) | Hoán vị hiện tại và đầu ra được tạo yêu cầu lưu trữ tuyến tính. | 

Kích thước đầu vào tối đa là rất nhỏ, vì vậy giải pháp tuyến tính này dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()

        t = int(sys.stdin.readline())
        ans = []

        for _ in range(t):
            n = int(sys.stdin.readline())
            p = list(map(int, sys.stdin.readline().split()))
            ans.append(" ".join(map(str, p)))

        sys.stdout.write("\n".join(ans))
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

assert run("""2
3
2 3 1
4
4 3 2 1
""") == """2 3 1
4 3 2 1""", "sample-style cases"

assert run("""1
3
1 2 3
""") == """1 2 3""", "minimum ordered permutation"

assert run("""1
5
5 4 3 2 1
""") == """5 4 3 2 1""", "reverse permutation"

assert run("""3
3
3 1 2
4
2 4 1 3
6
6 1 5 2 4 3
""") == """3 1 2
2 4 1 3
6 1 5 2 4 3""", "multiple cases"

assert run("""1
100
""" + " ".join(map(str, range(100, 0, -1))) + "\n") == """ + " ".join(map(str, range(100, 0, -1))), "maximum size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hoán vị có thứ tự | Trình tự có thứ tự giống nhau | Sao chép cơ bản | 
| Hoán vị ngược | Trình tự ngược lại tương tự | Không có sự sắp xếp ngẫu nhiên | 
| Nhiều trường hợp thử nghiệm | Mỗi trường hợp được in riêng | Xử lý vòng lặp đúng | 
| Hoán vị kích thước 100 | Trình tự hoàn toàn không thay đổi | Xử lý hạn chế tối đa | 

## Vỏ cạnh 

Đối với đầu vào hợp lệ nhỏ nhất:```
1
3
1 2 3
```thuật toán đọc ba giá trị vào`p`và ngay lập tức in chúng. Vì không cố gắng xây dựng lại nên hoán vị đã đúng vẫn không thay đổi. 

Đối với hoán vị giảm dần:```
1
5
5 4 3 2 1
```mảng được lưu trữ là`[5, 4, 3, 2, 1]`. Bước đầu ra lặp lại theo thứ tự hiện có, do đó nó in`5 4 3 2 1`. Giải pháp dựa trên sắp xếp sẽ thất bại vì nó sẽ phá hủy thông tin vị trí. 

Đối với nhiều trường hợp thử nghiệm:```
2
3
2 1 3
4
4 1 3 2
```vòng lặp xử lý hoán vị đầu tiên, nối thêm đầu ra của nó, sau đó xử lý hoán vị thứ hai một cách độc lập. Đầu ra cuối cùng giữ cả hai câu trả lời theo thứ tự giống như các trường hợp đầu vào. Điều này xác nhận rằng không có trạng thái nào từ trường hợp trước ảnh hưởng đến các trường hợp sau.
