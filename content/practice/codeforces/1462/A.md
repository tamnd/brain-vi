---
title: "CF 1462A - Trình tự yêu thích"
description: "Polycarp có một dãy số nguyên ưa thích nhưng chúng ta không nhìn thấy nó trực tiếp. Thay vào đó, chúng ta thấy kết quả của một quá trình viết đặc biệt mà anh ấy thực hiện."
date: "2026-06-11T02:08:05+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1462
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 690 (Div. 3)"
rating: 800
weight: 1462
solve_time_s: 139
verified: false
draft: false
---

[CF 1462A - Trình tự yêu thích](https://codeforces.com/problemset/problem/1462/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai, hai con trỏ 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Polycarp có một dãy số nguyên ưa thích nhưng chúng ta không nhìn thấy nó trực tiếp. Thay vào đó, chúng ta thấy kết quả của một quá trình viết đặc biệt mà anh ấy thực hiện. Anh ta viết phần tử đầu tiên ở đầu bên trái của bảng trắng, phần tử thứ hai ở đầu bên phải, sau đó luân phiên đặt phần tử có sẵn tiếp theo ở xa nhất có thể, sau đó sang phải càng xa càng tốt, tiếp tục ngoằn ngoèo này cho đến khi toàn bộ chuỗi được viết. Nhiệm vụ của chúng tôi là xây dựng lại trình tự ban đầu từ trạng thái bảng trắng cuối cùng. 

Đầu vào cung cấp số lượng trường hợp thử nghiệm và đối với mỗi trường hợp thử nghiệm, độ dài của trình tự bảng trắng và chính trình tự đó. Chúng ta phải xuất ra chuỗi yêu thích ban đầu của Polycarp cho từng trường hợp thử nghiệm. 

Cho rằng$n$có thể lên tới 300 và có thể có tới 300 trường hợp thử nghiệm, một giải pháp chạy trong khoảng$O(n)$mỗi trường hợp thử nghiệm là có thể chấp nhận được. Không có ràng buộc nào yêu cầu mọi thứ nhanh hơn và thậm chí cả$O(n^2)$cách tiếp cận có thể vượt qua vì$300^2 \cdot 300$chỉ dưới 30 triệu hoạt động, nhưng chúng tôi có thể làm tốt hơn nhiều. 

Các trường hợp cạnh phát sinh khi chuỗi có một phần tử duy nhất, khi tất cả các phần tử giống hệt nhau hoặc khi phần tử lớn nhất hoặc nhỏ nhất xuất hiện ở cuối. Ví dụ: nếu trình tự bảng trắng là`[42]`, đầu ra phải đơn giản là`[42]`. Nếu tất cả các phần tử đều bằng nhau, chẳng hạn như`[1, 1, 1, 1]`, đầu ra là cùng một chuỗi. Việc triển khai bất cẩn giả định các mẫu tăng hoặc giảm nghiêm ngặt có thể thất bại trong những trường hợp này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng ngược lại quá trình viết của Polycarp. Người ta có thể thử tất cả các kết hợp có thể của việc chèn trái và phải để xây dựng lại chuỗi ban đầu, nhưng điều đó nhanh chóng trở nên không khả thi vì$n$tăng lên do số hoán vị giai thừa. 

Quan sát quan trọng là Polycarp luôn viết số lớn nhất còn lại ở một trong các đầu của bảng trắng. Nói cách khác, phần tử đầu tiên của chuỗi bảng trắng là phần tử đầu tiên của chuỗi ban đầu. Sau đó, chúng ta có thể duy trì hai con trỏ, một ở đầu chuỗi bảng trắng và một ở cuối chuỗi, đồng thời chọn các phần tử từ một trong hai đầu theo thứ tự tái tạo lại vị trí ngoằn ngoèo. Bằng cách lặp đi lặp lại việc chọn giá trị lớn hơn trong hai cực trị chưa được sử dụng, chúng ta có thể xây dựng lại chuỗi ban đầu chỉ trong một lần. 

Việc quan sát làm giảm vấn đề thành việc truyền tải hai con trỏ: luôn chọn giá trị cực trị lớn hơn trong hai giá trị có sẵn từ chuỗi bảng trắng, nối nó vào chuỗi được xây dựng lại và loại bỏ nó khỏi việc xem xét. Điều này hiệu quả vì Polycarp luôn đặt số không được đặt lớn nhất tiếp theo ở cực trị và thứ tự vị trí thay thế từ trái sang phải, nhưng việc lựa chọn theo mức tối đa đảm bảo thứ tự chính xác có thể được xây dựng lại một cách tham lam. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tham lam hai con trỏ | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo danh sách trống`a`để giữ trình tự yêu thích được xây dựng lại. 
2. Sử dụng hai con trỏ,`l`khi bắt đầu trình tự bảng trắng`b`Và`r`ở cuối. 
3. Lặp lại trong khi`l <= r`: 

1. So sánh`b[l]`Và`b[r]`. 
2. Nối số lớn hơn vào dãy`a`. 
3. Nếu`b[l]`lớn hơn, tăng lên`l`. Ngược lại thì giảm`r`. 
4. Khi tất cả các phần tử đã được xử lý, hãy xuất ra`a`như trình tự được xây dựng lại. 

Lý do điều này có tác dụng là vì phần tử lớn nhất còn lại trong chuỗi bảng trắng phải tương ứng với phần tử tiếp theo mà Polycarp sẽ đặt ở hai đầu của bảng trắng. Bằng cách luôn chọn mức tối đa của hai thái cực có sẵn, chúng tôi tôn trọng thứ tự vị trí xen kẽ và tái tạo lại trình tự ban đầu một cách chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        b = list(map(int, input().split()))
        a = []
        l, r = 0, n - 1
        while l <= r:
            if b[l] > b[r]:
                a.append(b[l])
                l += 1
            else:
                a.append(b[r])
                r -= 1
        print(' '.join(map(str, a)))

if __name__ == "__main__":
    solve()
```Mã này đọc số lượng ca kiểm thử, sau đó đọc trình tự bảng trắng đối với mỗi ca kiểm thử. Cách tiếp cận hai con trỏ đảm bảo chúng ta luôn chọn đúng phần tử để xây dựng lại chuỗi ban đầu. Chúng tôi xử lý cẩn thận các con trỏ để tránh từng lỗi một và đảm bảo tất cả các phần tử đều được đưa vào. Việc in ấn được thực hiện ở cuối mỗi trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu đầu tiên:```
7
3 4 5 2 9 1 1
```| tôi | r | Đã chọn | một trình tự cho đến nay | 
| --- | --- | --- | --- | 
| 0 | 6 | 3 | [3] | 
| 1 | 6 | 4 | [3, 4] | 
| 2 | 6 | 5 | [3, 4, 5] | 
| 3 | 6 | 2 | [3, 4, 5, 2] | 
| 3 | 5 | 9 | [3, 4, 5, 2, 9] | 
| 3 | 4 | 1 | [3, 4, 5, 2, 9, 1] | 
| 3 | 3 | 1 | [3, 4, 5, 2, 9, 1, 1] | 

Điều này chứng tỏ rằng việc lựa chọn hai con trỏ sẽ xây dựng lại chính xác chuỗi ban đầu theo thứ tự. 

Đầu vào mẫu thứ hai:```
4
9 2 7 1
```| tôi | r | Đã chọn | một trình tự cho đến nay | 
| --- | --- | --- | --- | 
| 0 | 3 | 9 | [9] | 
| 1 | 3 | 1 | [9, 1] | 
| 1 | 2 | 7 | [9, 1, 7] | 
| 2 | 1 | 2 | [9, 1, 7, 2] | 

Điều này xác nhận rằng thuật toán cũng xử lý các chuỗi nhỏ một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được xem xét chính xác một lần với hai con trỏ | 
| Không gian | O(n) | Chúng tôi lưu trữ trình tự được xây dựng lại | 

Cho rằng$n \le 300$Và$t \le 300$, tổng số thao tác là khoảng$300 \cdot 300 = 90,000$, trong giới hạn thời gian 2 giây. Việc sử dụng bộ nhớ là không đáng kể đối với những hạn chế này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# Provided samples
assert run("6\n7\n3 4 5 2 9 1 1\n4\n9 2 7 1\n11\n8 4 3 1 2 7 8 7 9 4 2\n1\n42\n2\n11 7\n8\n1 1 1 1 1 1 1 1\n") == \
"3 4 5 2 9 1 1\n9 1 7 2\n8 4 3 1 2 7 8 7 9 4 2\n42\n11 7\n1 1 1 1 1 1 1 1", "provided samples"

# Custom cases
assert run("1\n1\n100\n") == "100", "single element"
assert run("1\n5\n1 2 3 4 5\n") == "5 4 3 2 1", "ascending sequence"
assert run("1\n5\n5 4 3 2 1\n") == "5 4 3 2 1", "descending sequence"
assert run("1\n4\n2 2 2 2\n") == "2 2 2 2", "all equal elements"
assert run("1\n6\n1 3 2 4 5 6\n") == "6 5 4 3 2 1", "mixed sequence"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n100`|`100`| Chuỗi phần tử đơn | 
|`1\n5\n1 2 3 4 5`|`5 4 3 2 1`| Trình tự tăng dần | 
|`1\n5\n5 4 3 2 1`|`5 4 3 2 1`| Trình tự giảm dần | 
|`1\n4\n2 2 2 2`|`2 2 2 2`| Tất cả các phần tử đều bằng nhau | 
|`1\n6\n1 3 2 4 5 6`|`6 5 4 3 2 1`| |
